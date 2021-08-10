这篇文章主要介绍Go内存分配和Go内存管理，会轻微涉及内存申请和释放，以及Go垃圾回收。

从非常宏观的角度看，Go的内存管理就是下图这个样子，我们今天主要关注其中标红的部分。

![image](https://user-images.githubusercontent.com/87457873/128820188-42407816-969d-46a9-88b1-27e0b3e18454.png)

本文基于go1.11.2，不同版本Go的内存管理可能存在差别，比如1.9与1.11的mheap定义就是差别比较大的，后续看源码的时候，请注意你的go版本，但无论你用哪个go版本，这都是一个优秀的资料，因为内存管理的思想和框架始终未变。

Go这门语言抛弃了C/C++中的开发者管理内存的方式：主动申请与主动释放，增加了逃逸分析和GC，将开发者从内存管理中释放出来，让开发者有更多的精力去关注软件设计，而不是底层的内存问题。这是Go语言成为高生产力语言的原因之一。

我们不需要精通内存的管理，因为它确实很复杂，但掌握内存的管理，可以让你写出更高质量的代码，另外，还能助你定位Bug。

这篇文章采用层层递进的方式，依次会介绍关于存储的基本知识，Go内存管理的“前辈”TCMalloc，然后是Go的内存管理和分配，最后是总结。这么做的目的是，希望各位能通过全局的认识和思考，拥有更好的编码思维和架构思维。

# 1. 存储基础知识回顾

这部分我们简单回顾一下计算机存储体系、虚拟内存、栈和堆，以及堆内存的管理，这部分内容对理解和掌握Go内存管理比较重要，建议忘记或不熟悉的朋友不要跳过。

## 存储金字塔

![image](https://user-images.githubusercontent.com/87457873/128820301-b3e88fe8-8ca3-4025-947e-d24c06ad9d07.png)

这幅图表达了计算机的存储体系，从上至下依次是：

* CPU寄存器
* Cache
* 内存
* 硬盘等辅助存储设备
* 鼠标等外接设备

从上至下，访问速度越来越慢，访问时间越来越长。

你有没有思考过下面2个简单的问题，如果没有不妨想想：

* 如果CPU直接访问硬盘，CPU能充分利用吗？
* 如果CPU直接访问内存，CPU能充分利用吗？

CPU速度很快，但硬盘等持久存储很慢，如果CPU直接访问磁盘，磁盘可以拉低CPU的速度，机器整体性能就会低下，为了弥补这2个硬件之间的速率差异，所以在CPU和磁盘之间增加了比磁盘快很多的内存。

![image](https://user-images.githubusercontent.com/87457873/128820371-63351524-fb2b-4d0a-bbc6-5b9f44f642f1.png)

然而，CPU跟内存的速率也不是相同的，从上图可以看到，CPU的速率提高的很快（摩尔定律），然而内存速率增长的很慢，虽然CPU的速率现在增加的很慢了，但是内存的速率也没增加多少，速率差距很大，从1980年开始CPU和内存速率差距在不断拉大，为了弥补这2个硬件之间的速率差异，所以在CPU跟内存之间增加了比内存更快的Cache，Cache是内存数据的缓存，可以降低CPU访问内存的时间。

不要以为有了Cache就万事大吉了，CPU的速率还在不断增大，Cache也在不断改变，从最初的1级，到后来的2级，到当代的3级Cache.

![image](https://user-images.githubusercontent.com/87457873/128820422-912aea9c-817c-4135-bfc2-31ff94de29f9.png)

三级Cache分别是L1、L2、L3，它们的速率是三个不同的层级，L1速率最快，与CPU速率最接近，是RAM速率的100倍，L2速率就降到了RAM的25倍，L3的速率更靠近RAM的速率。

看到这了，你有没有Get到整个存储体系的分层设计？自顶向下，速率越来越低，访问时间越来越长，从磁盘到CPU寄存器，上一层都可以看做是下一层的缓存。

看了分层设计，我们看一下内存，毕竟我们是介绍内存管理的文章。

## 虚拟内存
虚拟内存是当代操作系统必备的一项重要功能了，它向进程屏蔽了底层了RAM和磁盘，并向进程提供了远超物理内存大小的内存空间。我们看一下虚拟内存的分层设计。

![image](https://user-images.githubusercontent.com/87457873/128820453-eedf05d9-6bf5-4a10-a3a1-a3448adca3ff.png)

上图展示了某进程访问数据，当Cache没有命中的时候，访问虚拟内存获取数据的过程。

访问内存，实际访问的是虚拟内存，虚拟内存通过页表查看，当前要访问的虚拟内存地址，是否已经加载到了物理内存，如果已经在物理内存，则取物理内存数据，如果没有对应的物理内存，则从磁盘加载数据到物理内存，并把物理内存地址和虚拟内存地址更新到页表。

有没有Get到：物理内存就是磁盘存储缓存层。

另外，在没有虚拟内存的时代，物理内存对所有进程是共享的，多进程同时访问同一个物理内存存在并发访问问题。引入虚拟内存后，每个进程都要各自的虚拟内存，内存的并发访问问题的粒度从多进程级别，可以降低到多线程级别。

## 栈和堆

我们现在从虚拟内存，再进一层，看虚拟内存中的栈和堆，也就是进程对内存的管理。

![image](https://user-images.githubusercontent.com/87457873/128820485-6fe0ff08-c9f3-40fd-b2cb-73dd202675ec.png)

上图展示了一个进程的虚拟内存划分，代码中使用的内存地址都是虚拟内存地址，而不是实际的物理内存地址。栈和堆只是虚拟内存上2块不同功能的内存区域：

* 栈在高地址，从高地址向低地址增长。
* 堆在低地址，从低地址向高地址增长。

栈和堆相比有这么几个好处：

1、栈的内存管理简单，分配比堆上快。
2、栈的内存不需要回收，而堆需要，无论是主动free，还是被动的垃圾回收，这都需要花费额外的CPU。<br>
3、栈上的内存有更好的局部性，堆上内存访问就不那么友好了，CPU访问的2块数据可能在不同的页上，CPU访问数据的时间可能就上去了。<br>

## 堆内存管理

![image](https://user-images.githubusercontent.com/87457873/128820577-e3041419-8a54-43ad-9688-c965439261dc.png)

我们再进一层，当我们说内存管理的时候，主要是指堆内存的管理，因为栈的内存管理不需要程序去操心。这小节看下堆内存管理干的是啥，如上图所示主要是3部分：分配内存块，回收内存块和组织内存块。

在一个最简单的内存管理中，堆内存最初会是一个完整的大块，即未分配内存，当来申请的时候，就会从未分配内存，分割出一个小内存块(block)，然后用链表把所有内存块连接起来。需要一些信息描述每个内存块的基本信息，比如大小(size)、是否使用中(used)和下一个内存块的地址(next)，内存块实际数据存储在data中。

![image](https://user-images.githubusercontent.com/87457873/128820621-5b78d214-e451-40d0-aded-4fd065300e64.png)

一个内存块包含了3类信息，如下图所示，元数据、用户数据和对齐字段，内存对齐是为了提高访问效率。下图申请5Byte内存的时候，就需要进行内存对齐。

![image](https://user-images.githubusercontent.com/87457873/128820641-b9725bd9-ddc7-4c0d-ae28-ca6a8b5595bc.png)

释放内存实质是把使用的内存块从链表中取出来，然后标记为未使用，当分配内存块的时候，可以从未使用内存块中有先查找大小相近的内存块，如果找不到，再从未分配的内存中分配内存。

上面这个简单的设计中还没考虑内存碎片的问题，因为随着内存不断的申请和释放，内存上会存在大量的碎片，降低内存的使用率。为了解决内存碎片，可以将2个连续的未使用的内存块合并，减少碎片。

以上就是内存管理的基本思路，关于基本的内存管理，想了解更多，可以阅读这篇文章《Writing a Memory Allocator》，本节的3张图片也是来自这片文章。

# 2. TCMalloc
TCMalloc是Thread Cache Malloc的简称，是Go内存管理的起源，Go的内存管理是借鉴了TCMalloc，随着Go的迭代，Go的内存管理与TCMalloc不一致地方在不断扩大，但其主要思想、原理和概念都是和TCMalloc一致的，如果跳过TCMalloc直接去看Go的内存管理，也许你会似懂非懂。

掌握TCMalloc的理念，无需去关注过多的源码细节，就可以为掌握Go的内存管理打好基础，基础打好了，后面知识才扎实。

在Linux里，其实有不少的内存管理库，比如glibc的ptmalloc，FreeBSD的jemalloc，Google的tcmalloc等等，为何会出现这么多的内存管理库？本质都是在多线程编程下，追求更高内存管理效率：更快的分配是主要目的。

那如何更快的分配内存？

我们前面提到：

> 引入虚拟内存后，让内存的并发访问问题的粒度从多进程级别，降低到多线程级别。

这是更快分配内存的第一个层次。

同一进程的所有线程共享相同的内存空间，他们申请内存时需要加锁，如果不加锁就存在同一块内存被2个线程同时访问的问题。

TCMalloc的做法是什么呢？为每个线程预分配一块缓存，线程申请小内存时，可以从缓存分配内存，这样有2个好处：

1、为线程预分配缓存需要进行1次系统调用，后续线程申请小内存时，从缓存分配，都是在用户态执行，没有系统调用，缩短了内存总体的分配和释放时间，这是快速分配内存的第二个层次。<br>
2、多个线程同时申请小内存时，从各自的缓存分配，访问的是不同的地址空间，无需加锁，把内存并发访问的粒度进一步降低了，这是快速分配内存的第三个层次。<br>

## 基本原理

下面就简单介绍下TCMalloc，细致程度够我们理解Go的内存管理即可。

> 声明：我没有研究过TCMalloc，以下介绍根据TCMalloc官方资料和其他博主资料总结而来，错误之处请朋友告知我。

![image](https://user-images.githubusercontent.com/87457873/128820791-5eb728e4-44e3-46fd-a9c6-66b3974c78b9.png)

结合上图，介绍TCMalloc的几个重要概念：

* Page：操作系统对内存管理以页为单位，TCMalloc也是这样，只不过TCMalloc里的Page大小与操作系统里的大小并不一定相等，而是倍数关系。《TCMalloc解密》里称x64下Page大小是8KB。
* Span：一组连续的Page被称为Span，比如可以有2个页大小的Span，也可以有16页大小的Span，Span比Page高一个层级，是为了方便管理一定大小的内存区域，Span是TCMalloc中内存管理的基本单位。
* ThreadCache：每个线程各自的Cache，一个Cache包含多个空闲内存块链表，每个链表连接的都是内存块，同一个链表上内存块的大小是相同的，也可以说按内存块大小，给内存块分了个类，这样可以根据申请的内存大小，快速从合适的链表选择空闲内存块。由于每个线程有自己的ThreadCache，所以ThreadCache访问是无锁的。
* CentralCache：是所有线程共享的缓存，也是保存的空闲内存块链表，链表的数量与ThreadCache中链表数量相同，当ThreadCache内存块不足时，可以从CentralCache取，当ThreadCache内存块多时，可以放回CentralCache。由于CentralCache是共享的，所以它的访问是要加锁的。
* PageHeap：PageHeap是堆内存的抽象，PageHeap存的也是若干链表，链表保存的是Span，当CentralCache没有内存的时，会从PageHeap取，把1个Span拆成若干内存块，添加到对应大小的链表中，当CentralCache内存多的时候，会放回PageHeap。如下图，分别是1页Page的Span链表，2页Page的Span链表等，最后是large span set，这个是用来保存中大对象的。毫无疑问，PageHeap也是要加锁的。

![image](https://user-images.githubusercontent.com/87457873/128820841-34618c85-8d60-4e9d-9dba-55e0cee81986.png)

上文提到了小、中、大对象，Go内存管理中也有类似的概念，我们瞄一眼TCMalloc的定义：

* 小对象大小：0~256KB
* 中对象大小：257~1MB
* 大对象大小：>1MB

小对象的分配流程：ThreadCache -> CentralCache -> HeapPage，大部分时候，ThreadCache缓存都是足够的，不需要去访问CentralCache和HeapPage，无锁分配加无系统调用，分配效率是非常高的。

中对象分配流程：直接在PageHeap中选择适当的大小即可，128 Page的Span所保存的最大内存就是1MB。

大对象分配流程：从large span set选择合适数量的页面组成span，用来存储数据。

# 3. Go内存管理

## Go内存管理的基本概念

前面计算机基础知识回顾，是一种自上而下，从宏观到微观的介绍方式，把目光引入到今天的主题。

Go内存管理的许多概念在TCMalloc中已经有了，含义是相同的，只是名字有一些变化。先给大家上一幅宏观的图，借助图一起来介绍。

![image](https://user-images.githubusercontent.com/87457873/128821054-6f48a32f-34a3-494c-aedc-d079ba782c1c.png)

### Page
与TCMalloc中的Page相同，x64下1个Page的大小是8KB。上图的最下方，1个浅蓝色的长方形代表1个Page。

### Span
与TCMalloc中的Span相同，Span是内存管理的基本单位，代码中为mspan，一组连续的Page组成1个Span，所以上图一组连续的浅蓝色长方形代表的是一组Page组成的1个Span，另外，1个淡紫色长方形为1个Span。

### mcache
mcache与TCMalloc中的ThreadCache类似，mcache保存的是各种大小的Span，并按Span class分类，小对象直接从mcache分配内存，它起到了缓存的作用，并且可以无锁访问。

但mcache与ThreadCache也有不同点，TCMalloc中是每个线程1个ThreadCache，Go中是每个P拥有1个mcache，因为在Go程序中，当前最多有GOMAXPROCS个线程在用户态运行，所以最多需要GOMAXPROCS个mcache就可以保证各线程对mcache的无锁访问，线程的运行又是与P绑定的，把mcache交给P刚刚好。

### mcentral
mcentral与TCMalloc中的CentralCache类似，是所有线程共享的缓存，需要加锁访问，它按Span class对Span分类，串联成链表，当mcache的某个级别Span的内存被分配光时，它会向mcentral申请1个当前级别的Span。

但mcentral与CentralCache也有不同点，CentralCache是每个级别的Span有1个链表，mcache是每个级别的Span有2个链表，这和mcache申请内存有关，稍后我们再解释。

### mheap
mheap与TCMalloc中的PageHeap类似，它是堆内存的抽象，把从OS申请出的内存页组织成Span，并保存起来。当mcentral的Span不够用时会向mheap申请，mheap的Span不够用时会向OS申请，向OS的内存申请是按页来的，然后把申请来的内存页生成Span组织起来，同样也是需要加锁访问的。

但mheap与PageHeap也有不同点：mheap把Span组织成了树结构，而不是链表，并且还是2棵树，然后把Span分配到heapArena进行管理，它包含地址映射和span是否包含指针等位图，这样做的主要原因是为了更高效的利用内存：分配、回收和再利用。

### 大小转换
除了以上内存块组织概念，还有几个重要的大小概念，一定要拿出来讲一下，不要忽视他们的重要性，他们是内存分配、组织和地址转换的基础。

![image](https://user-images.githubusercontent.com/87457873/128821092-e3743fd7-fecf-4f14-bd4a-c1db5187e6fd.png)

* object size：代码里简称size，指申请内存的对象大小。
* size class：代码里简称class，它是size的级别，相当于把size归类到一定大小的区间段，比如size[1,8]属于size class 1，size(8,16]属于size class 2。
* span class：指span的级别，但span class的大小与span的大小并没有正比关系。span class主要用来和size class做对应，1个size class对应2个span class，2个span class的span大小相同，只是功能不同，1个用来存放包含指针的对象，一个用来存放不包含指针的对象，不包含指针对象的Span就无需GC扫描了。
* num of page：代码里简称npage，代表Page的数量，其实就是Span包含的页数，用来分配内存。

在介绍这几个大小之间的换算前，我们得先看下图这个表，这个表决定了映射关系。

最上面2行是我手动加的，前3列分别是size class，object size和span size，根据这3列做size、size class和num of page之间的转换。

仔细看一遍这个表，再向下看转换是如何实现的。

![image](https://user-images.githubusercontent.com/87457873/128821146-82f3a3f0-35a9-4c98-bf6f-b349813ae482.png)

在Go内存大小转换那幅图中已经标记各大小之间的转换，分别是数组：class_to_size，size_to_class*和class_to_allocnpages，这3个数组内容，就是跟上表的映射关系匹配的。比如class_to_size，从上表看class 1对应的保存对象大小为8，所以class_to_size[1]=8，span大小为8192Byte，即8KB，为1页，所以class_to_allocnpages[1]=1。

![image](https://user-images.githubusercontent.com/87457873/128821185-0019567b-eb80-40d9-b385-d37d7198366f.png)

为何不使用函数计算各种转换，而是写成数组？

有1个很重要的原因：空间换时间。你如果仔细观察了，上表中的转换，并不能通过简单的公式进行转换，比如size和size class的关系，并不是正比的。这些数据是使用较复杂的公式计算出来的，公式在makesizeclass.go中，这其中存在指数运算与for循环，造成每次大小转换的时间复杂度为O(N*2^N)。另外，对一个程序而言，内存的申请和管理操作是很多的，如果不能快速完成，就是非常的低效。把以上大小转换写死到数组里，做到了把大小转换的时间复杂度直接降到O(1)。

### 其他转换表字段
第4列num of objects代表是当前size class级别的Span可以保存多少对象数量，第5列tail waste是span%obj计算的结果，因为span的大小并不一定是对象大小的整数倍。

最后一列max waste代表最大浪费的内存百分比，计算方法在printComment函数中:

```go
func printComment(w io.Writer, classes []class) {
	fmt.Fprintf(w, "// %-5s  %-9s  %-10s  %-7s  %-10s  %-9s\n", "class", "bytes/obj", "bytes/span", "objects", "tail waste", "max waste")
	prevSize := 0
	for i, c := range classes {
		if i == 0 {
			continue
		}
		spanSize := c.npages * pageSize
		objects := spanSize / c.size
		tailWaste := spanSize - c.size*(spanSize/c.size)
		maxWaste := float64((c.size-prevSize-1)*objects+tailWaste) / float64(spanSize)
		prevSize = c.size
		fmt.Fprintf(w, "// %5d  %9d  %10d  %7d  %10d  %8.2f%%\n", i, c.size, spanSize, objects, tailWaste, 100*maxWaste)
	}
	fmt.Fprintf(w, "\n")
}
```

Span最浪费内存的场景是：Span内的每一个对象空间保存的对象，实际占用内存是前一个class中对象的大小加1，这样无法占用低一级的Span。一个对象空间未被占用的内存就被浪费了，所以一个Span内对象空间所浪费的内存为：所有对象空间浪费的内存之和+tail waste。

    ((c.size - (preSize+1)) * objects + tailWaste) / spanSize

![image](https://user-images.githubusercontent.com/87457873/128821277-c482695c-b31e-4d0a-83f5-bc9ede106a28.png)

## Go内存分配

涉及的概念已经讲完了，我们看下Go内存分配原理。

Go中的内存分类并不像TCMalloc那样分成小、中、大对象，但是它的小对象里又细分了一个Tiny对象，Tiny对象指大小在1Byte到16Byte之间并且不包含指针的对象。小对象和大对象只用大小划定，无其他区分。

![image](https://user-images.githubusercontent.com/87457873/128821327-ab48c567-5717-4007-bc5f-c6a7e571a340.png)

小对象是在mcache中分配的，而大对象是直接从mheap分配的，从小对象的内存分配看起。

### 小对象分配

![image](https://user-images.githubusercontent.com/87457873/128821352-23820872-97e6-4752-9190-b05c31397d8d.png)

#### 为对象寻找span

寻找span的流程如下：

* 计算对象所需内存大小size
* 根据size到size class映射，计算出所需的size class
* 根据size class和对象是否包含指针计算出span class
* 获取该span class指向的span。
* 以分配一个不包含指针的，大小为24Byte的对象为例。

根据映射表：

```
// class  bytes/obj  bytes/span  objects  tail waste  max waste
//     1          8        8192     1024           0     87.50%
//     2         16        8192      512           0     43.75%
//     3         32        8192      256           0     46.88%
//     4         48        8192      170          32     31.52%

```
size class 3，它的对象大小范围是(16,32]Byte，24Byte刚好在此区间，所以此对象的size class为3。

Size class到span class的计算如下：

```go
// noscan为true代表对象不包含指针
func makeSpanClass(sizeclass uint8, noscan bool) spanClass {
	return spanClass(sizeclass<<1) | spanClass(bool2int(noscan))
}
```

所以，对应的span class为：

```go
span class = 3 << 1 | 1 = 7
```

所以该对象需要的是span class 7指向的span。

#### 从span分配对象空间
Span可以按对象大小切成很多份，这些都可以从映射表上计算出来，以size class 3对应的span为例，span大小是8KB，每个对象实际所占空间为32Byte，这个span就被分成了256块，可以根据span的起始地址计算出每个对象块的内存地址。

![image](https://user-images.githubusercontent.com/87457873/128822002-693fef40-4f44-4f49-b46e-3743d7a511bd.png)

随着内存的分配，span中的对象内存块，有些被占用，有些未被占用，比如上图，整体代表1个span，蓝色块代表已被占用内存，绿色块代表未被占用内存。

当分配内存时，只要快速找到第一个可用的绿色块，并计算出内存地址即可，如果需要还可以对内存块数据清零。

#### span没有空间怎么分配对象
span内的所有内存块都被占用时，没有剩余空间继续分配对象，mcache会向mcentral申请1个span，mcache拿到span后继续分配对象。

#### mcentral向mcache提供span
mcentral和mcache一样，都是0~133这134个span class级别，但每个级别都保存了2个span list，即2个span链表：

* nonempty：这个链表里的span，所有span都至少有1个空闲的对象空间。这些span是mcache释放span时加入到该链表的。
* empty：这个链表里的span，所有的span都不确定里面是否有空闲的对象空间。当一个span交给mcache的时候，就会加入到empty链表。

这2个东西名称一直有点绕，建议直接把empty理解为没有对象空间就好了。

![image](https://user-images.githubusercontent.com/87457873/128822110-dbf16d7a-bb9b-4221-9ac5-bb644dde5848.png)

实际代码中每1个span class对应1个mcentral，图里把所有mcentral抽象成1个整体了。

mcache向mcentral要span时，mcentral会先从nonempty搜索满足条件的span，如果没有找到再从emtpy搜索满足条件的span，然后把找到的span交给mcache。

#### mheap的span管理
mheap里保存了2棵二叉排序树，按span的page数量进行排序：

* free：free中保存的span是空闲并且非垃圾回收的span。
* scav：scav中保存的是空闲并且已经垃圾回收的span。

如果是垃圾回收导致的span释放，span会被加入到scav，否则加入到free，比如刚从OS申请的的内存也组成的Span。

![image](https://user-images.githubusercontent.com/87457873/128822168-91ebb4b6-4984-4cb8-b231-ad71b666be1e.png)

mheap中还有arenas，有一组heapArena组成，每一个heapArena都包含了连续的pagesPerArena个span，这个主要是为mheap管理span和垃圾回收服务。

mheap本身是一个全局变量，它其中的数据，也都是从OS直接申请来的内存，并不在mheap所管理的那部分内存内。

#### mcentral向mheap要span
mcentral向mcache提供span时，如果emtpy里也没有符合条件的span，mcentral会向mheap申请span。

mcentral需要向mheap提供需要的内存页数和span class级别，然后它优先从free中搜索可用的span，如果没有找到，会从scav中搜索可用的span，如果还没有找到，它会向OS申请内存，再重新搜索2棵树，必然能找到span。如果找到的span比需求的span大，则把span进行分割成2个span，其中1个刚好是需求大小，把剩下的span再加入到free中去，然后设置需求span的基本信息，然后交给mcentral。

#### mheap向OS申请内存
当mheap没有足够的内存时，mheap会向OS申请内存，把申请的内存页保存到span，然后把span插入到free树 。

在32位系统上，mheap还会预留一部分空间，当mheap没有空间时，先从预留空间申请，如果预留空间内存也没有了，才向OS申请。

### 大对象分配
大对象的分配比小对象省事多了，99%的流程与mcentral向mheap申请内存的相同，所以不重复介绍了，不同的一点在于mheap会记录一点大对象的统计信息，见mheap.alloc_m()。

## Go垃圾回收和内存释放

如果只申请和分配内存，内存终将枯竭，Go使用垃圾回收收集不再使用的span，调用mspan.scavenge()把span释放给OS（并非真释放，只是告诉OS这片内存的信息无用了，如果你需要的话，收回去好了），然后交给mheap，mheap对span进行span的合并，把合并后的span加入scav树中，等待再分配内存时，由mheap进行内存再分配，Go垃圾回收也是一个很强的主题，计划后面单独写一篇文章介绍。

现在我们关注一下，Go程序是怎么把内存释放给操作系统的？

释放内存的函数是sysUnused，它会被mspan.scavenge()调用:
```go
// MAC下的实现
func sysUnused(v unsafe.Pointer, n uintptr) {
	// MADV_FREE_REUSABLE is like MADV_FREE except it also propagates
	// accounting information about the process to task_info.
	madvise(v, n, _MADV_FREE_REUSABLE)
}
```
注释说_MADV_FREE_REUSABLE与MADV_FREE的功能类似，它的功能是给内核提供一个建议：这个内存地址区间的内存已经不再使用，可以回收。但内核是否回收，以及什么时候回收，这就是内核的事情了。如果内核真把这片内存回收了，当Go程序再使用这个地址时，内核会重新进行虚拟地址到物理地址的映射。所以在内存充足的情况下，内核也没有必要立刻回收内存。

# 4. Go栈内存
最后提一下栈内存。从一个宏观的角度看，内存管理不应当只有堆，也应当有栈。

每个goroutine都有自己的栈，栈的初始大小是2KB，100万的goroutine会占用2G，但goroutine的栈会在2KB不够用时自动扩容，当扩容为4KB的时候，百万goroutine会占用4GB。

关于goroutine栈内存管理，有篇很好的文章，饿了么框架技术部的专栏文章：《聊一聊goroutine stack》，把里面的一段内容摘录下，你感受下：

>可以看到在rpc调用(grpc invoke)时，栈会发生扩容(runtime.morestack)，也就意味着在读写routine内的任何rpc调用都会导致栈扩容， 占用的内存空间会扩大为原来的两倍，4kB的栈会变为8kB，100w的连接的内存占用会从8G扩大为16G（全双工，不考虑其他开销），这简直是噩梦。

另外，再推荐一篇曹大翻译的一篇汇编入门文章，里面也介绍了扩栈：第一章: Go 汇编入门 ，顺便入门一下汇编。

5. 总结
内存分配原理就不再回顾了，强调2个重要的思想：

* 使用缓存提高效率。在存储的整个体系中到处可见缓存的思想，Go内存分配和管理也使用了缓存，利用缓存一是减少了系统调用的次数，二是降低了锁的粒度，减少加锁的次数，从这2点提高了内存管理效率。
* 以空间换时间，提高内存管理效率。空间换时间是一种常用的性能优化思想，这种思想其实非常普遍，比如Hash、Map、二叉排序树等数据结构的本质就是空间换时间，在数据库中也很常见，比如数据库索引、索引视图和数据缓存等，再如Redis等缓存数据库也是空间换时间的思想。




























