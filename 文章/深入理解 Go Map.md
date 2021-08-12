哈希表的意义不言而喻，它能提供 O(1) 复杂度的读写性能，所以主流编程语言中都内置有哈希表。

哈希表的关键在于哈希函数， 好的哈希函数能减少哈希碰撞，提供最优秀的读写性能。

# 哈希碰撞
因为没有完美的哈希函数， 所以哈希碰撞不可避免，一般有开放寻址法和拉链法，其中拉链法是主流

* 开放寻址法：当向哈希表写入新的数据时，如果发生了冲突，就会将键值对写入到下一个索引不为空的位置

![image](https://user-images.githubusercontent.com/87457873/129164275-f6c252fe-406f-4d07-8024-ba99dc7f9524.png)

* 拉链法：拉链法一般使用数组和链表组成，数据经过哈希函数得到一个桶时，先遍历桶中的链表，存在相同的键值对，则更新，不存在则在链表末尾追加新键值对

![image](https://user-images.githubusercontent.com/87457873/129164344-29818990-fea2-4552-acab-a07d53b5c3b3.png)

# Go 表示哈希表的数据结构
```go
type hmap struct {
  // 表示哈希表中元素的数量
	count     int
  flags     uint8
  
  // 表示哈希表中桶的数量， len(buckets) = 2^B
	B         uint8
  noverflow uint16
  
  // hash函数的种子
  hash0     uint32

	buckets    unsafe.Pointer
  
  // 用于在扩容时保存之前 buckets
  // 因为每次扩容都是2的倍数，所以 bucket = 2oldbuckets
	oldbuckets unsafe.Pointer
	nevacuate  uintptr

	extra *mapextra
}

type mapextra struct {
	overflow    *[]*bmap
	oldoverflow *[]*bmap
	nextOverflow *bmap
}
```
![image](https://user-images.githubusercontent.com/87457873/129164391-7cae687d-2b78-4fcd-ae29-79d741db5dfd.png)

哈希表 hmap 的桶是 bmap，每个 bmap 都能存储 8 个键值对，单个桶装满时会使用 nextOverflow 桶存储溢出的数据

```go
type bmap struct {
	// 存储了键的哈希的高 8 位
  // 通过比较不同键的哈希的高 8 位可以减少访问键值对次数以提高性能
  tophash [bucketCnt]uint8
}
```

# 访问 map 中的数据

![image](https://user-images.githubusercontent.com/87457873/129164455-18812f73-6f36-425a-ae5b-c75620fa3711.png)

如上图所示，每一个桶都是一整片的内存空间，当发现桶中的 tophash 与传入键的 tophash 匹配之后，我们会通过指针和偏移量获取哈希中存储的键 keys[0] 并与 key 比较，如果两者相同就会获取目标值的指针 values[0] 并返回

# 向 map 写入数据

![image](https://user-images.githubusercontent.com/87457873/129164489-5d5bf08d-5db1-40fa-b418-b1b905c7225c.png)

函数会根据传入的键拿到对应的哈希和桶，通过遍历比较桶中存储的 tophash 和键的哈希，如果找到了相同结果就会返回目标位置的地址，获得目标地址之后会通过算术计算寻址获得键值对 k 和 val， 如果当前键值对在哈希中不存在，哈希会为新键值对规划存储的内存地址，这期间只会返回内存地址，真正的赋值操作是在编译期间插入的。

    00018 (+5) CALL runtime.mapassign_fast64(SB)
    00020 (5) MOVQ 24(SP), DI               ;; DI = &value
    00026 (5) LEAQ go.string."88"(SB), AX   ;; AX = &"88"
    00027 (5) MOVQ AX, (DI)                 ;; *DI = AX
    
我们通过 LEAQ 指令将字符串的地址存储到寄存器 AX 中，MOVQ 指令将字符串 "88" 存储到了目标地址上完成了这次哈希的写入

# 扩容

随着哈希表中元素的逐渐增加，哈希表的性能会逐渐恶化，当装载因子 > 6.5 时， 或者 哈希表创建了太多的溢出桶， 会触发扩容

>装载因子 = 元素数量 / 桶数量

哈希表在扩容的过程中会创建一组新桶和溢出桶，随后将原油的桶数组设置到 oldbuckets 上，将新桶设置到 buckets 上，新计算旧桶内元素的哈希到新桶上，

在扩容期间访问哈希表时会使用旧桶，整个期间元素再分配的过程也是在调用写操作时增量进行的，不会造成性能的瞬时巨大抖动

