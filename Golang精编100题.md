## 能力模型

| 级别                                                         | 模型                               |
| ------------------------------------------------------------ | ---------------------------------- |
| 初级                                                         |                                    |
| primary                                                      | 熟悉基本语法，能够看懂代码的意图； |
| 在他人指导下能够完成用户故事的开发，编写的代码符合CleanCode规范； |                                    |
| 中级                                                         |                                    |
| intermediate                                                 | 能够独立完成用户故事的开发和测试； |
| 能够嗅出代码的坏味道，并知道如何重构达成目标；               |                                    |
| 高级                                                         |                                    |
| senior                                                       | 能够开发出高质量高性能的代码；     |
| 能够熟练使用高级特性，开发编程框架或测试框架；               |                                    |

## 选择题

### 1、**[primary]** 下面属于关键字的是（）

A. func

B. def

C. struct

D. class

参考答案：AC

### 2、**[primary]** 定义一个包内全局字符串变量，下面语法正确的是 （）

A. var str string

B. str := ""

C. str = ""

D. var str = ""

参考答案：AD

### 3、**[primary]** 通过指针变量 p 访问其成员变量 name，下面语法正确的是（）

A. p.name

B. (*p).name

C. (&p).name

D. p->name

参考答案：AB

### 4、**[primary]** 关于接口和类的说法，下面说法正确的是（）

A. 一个类只需要实现了接口要求的所有函数，我们就说这个类实现了该接口

B. 实现类的时候，只需要关心自己应该提供哪些方法，不用再纠结接口需要拆得多细才合理

C. 类实现接口时，需要导入接口所在的包

D. 接口由使用方按自身需求来定义，使用方无需关心是否有其他模块定义过类似的接口

参考答案：ABD

### 5、**[primary]** 关于字符串连接，下面语法正确的是（）

A. str := ‘abc’ + ‘123’

B. str := "abc" + "123"

C. str ：= '123' + "abc"

D. fmt.Sprintf("abc%d", 123)

参考答案：BD

### 6、**[primary]** 关于协程，下面说法正确是（）

A. 协程和线程都可以实现程序的并发执行

B. 线程比协程更轻量级

C. 协程不存在死锁问题

D. 通过channel来进行协程间的通信

参考答案：AD

### 7、**[intermediate]** 关于init函数，下面说法正确的是（）

A. 一个包中，可以包含多个init函数

B. 程序编译时，先执行导入包的init函数，再执行本包内的init函数

C. main包中，不能有init函数

D. init函数可以被其他函数调用

参考答案：AB

### 8、**[primary]** 关于循环语句，下面说法正确的有（）

A. 循环语句既支持for关键字，也支持while和do-while

B. 关键字for的基本使用方法与C/C++中没有任何差异

C. for循环支持continue和break来控制循环，但是它提供了一个更高级的break，可以选择中断哪一个循环

D. for循环不支持以逗号为间隔的多个赋值语句，必须使用平行赋值的方式来初始化多个变量

参考答案：CD

### 9、**[intermediate]** 对于函数定义：

```go
func add(args ...int) int {
        sum := 0
        for _, arg := range args {
            sum += arg
        }
        return sum
}
```

```go
下面对add函数调用正确的是（）
A. add(1, 2)
B. add(1, 3, 7)
C. add([]int{1, 2})
D. add([]int{1, 3, 7}...)
```

参考答案：ABD

### 10、**[primary]** 关于类型转化，下面语法正确的是（）

A.

```GO
type MyInt int
var i int = 1
var j MyInt = i
```

B.

```GO
type MyInt int
var i int = 1
var j MyInt = (MyInt)i
```

C.

```GO
type MyInt int
var i int = 1
var j MyInt = MyInt(i)
```

D.

```GO
type MyInt int
var i int = 1
var j MyInt = i.(MyInt)
```

参考答案：C

### 11、**[primary]** 关于局部变量的初始化，下面正确的使用方式是（）

A. var i int = 10

B. var i = 10

C. i := 10

D. i = 10

参考答案：ABC

### 12、**[primary]** 关于const常量定义，下面正确的使用方式是（）

A.

```GO
const Pi float64 = 3.14159265358979323846
const zero = 0.0
```

B.

```GO
const (
        size int64 = 1024
        eof = -1
)
```

C.

```GO
const (
        ERR_ELEM_EXIST error = errors.New("element already exists")
        ERR_ELEM_NT_EXIST error = errors.New("element not exists")
)
```

D.

```GO
const u, v float32 = 0, 3
const a, b, c = 3, 4, "foo"
```

参考答案：ABD

### 13、**[primary]** 关于布尔变量b的赋值，下面错误的用法是（）

A. b = true

B. b = 1

C. b = bool(1)

D. b = (1 == 2)

参考答案：BC

### 14、**[intermediate]** 下面的程序的运行结果是（）



```GO
func main() {
        if (true) {
            defer fmt.Printf("1")
        } else {
            defer fmt.Printf("2")
        }
        fmt.Printf("3")
}
```

A. 321

B. 32

C. 31

D. 13

参考答案：C

### 15、**[primary]** 关于switch语句，下面说法正确的有（）

A. 条件表达式必须为常量或者整数

B. 单个case中，可以出现多个结果选项

C. 需要用break来明确退出一个case

D. 只有在case中明确添加fallthrough关键字，才会继续执行紧跟的下一个case

参考答案：BD

### 16、**[intermediate]** golang中没有隐藏的this指针，这句话的含义是（）

A. 方法施加的对象显式传递，没有被隐藏起来

B. golang沿袭了传统面向对象编程中的诸多概念，比如继承、虚函数和构造函数

C. golang的面向对象表达更直观，对于面向过程只是换了一种语法形式来表达

D. 方法施加的对象不需要非得是指针，也不用非得叫this

参考答案：ACD

### 17、**[intermediate]** golang中的引用类型包括（）

A. 数组切片

B. map

C. channel

D. interface

参考答案：ABCD

### 18、**[intermediate]** golang中的指针运算包括（）

A. 可以对指针进行自增或自减运算

B. 可以通过“&”取指针的地址

C. 可以通过“*”取指针指向的数据

D. 可以对指针进行下标运算

参考答案：BC

### 19、**[primary]** 关于main函数（可执行程序的执行起点），下面说法正确的是（）

A. main函数不能带参数

B. main函数不能定义返回值

C. main函数所在的包必须为main包

D. main函数中可以使用flag包来获取和解析命令行参数

参考答案：ABCD

### 20、**[intermediate]** 下面赋值正确的是（）

A. var x = nil

B. var x interface{} = nil

C. var x string = nil

D. var x error = nil

参考答案：BD



### 21、**[intermediate]** 关于整型切片的初始化，下面正确的是（）

A. s := make([]int)

B. s := make([]int, 0)

C. s := make([]int, 5, 10)

D. s := []int{1, 2, 3, 4, 5}



参考答案：BCD



### 22、**[intermediate]** 从切片中删除一个元素，下面的算法实现正确的是（）

A.

```GO
func (s *Slice)Remove(value interface{}) error {
        for i, v := range *s {
            if isEqual(value, v) {
                if i== len(*s) - 1 {
                    *s = (*s)[:i]
                }else {
                    *s = append((*s)[:i],(*s)[i + 2:]...)
                }
                return nil
            }
        }
        return ERR_ELEM_NT_EXIST
}
```



B.

```GO
func (s *Slice)Remove(value interface{}) error {
        for i, v := range *s {
            if isEqual(value, v) {
                *s = append((*s)[:i],(*s)[i + 1:])
                return nil
            }
        }
        return ERR_ELEM_NT_EXIST
}
```



C.

```GO
func (s *Slice)Remove(value interface{}) error {
        for i, v := range *s {
            if isEqual(value, v) {
                delete(*s, v)
                return nil
            }
        }
        return ERR_ELEM_NT_EXIST
}
```



D.

```GO
func (s *Slice)Remove(value interface{}) error {
        for i, v := range *s {
            if isEqual(value, v) {
                *s = append((*s)[:i],(*s)[i + 1:]...)
                return nil
            }
        }
        return ERR_ELEM_NT_EXIST
}
```



参考答案：D



### 23、**[primary]** 对于局部变量整型切片x的赋值，下面定义正确的是（）

A.

```GO
x := []int{
        1, 2, 3,
        4, 5, 6,
}
```



B.

```go
x := []int{
        1, 2, 3,
        4, 5, 6
}
```



C.



```GO
x := []int{
        1, 2, 3,
        4, 5, 6}
```



D.



```GO
x := []int{1, 2, 3, 4, 5, 6,}
```



参考答案：ACD



### 24、**[primary]** 关于变量的自增和自减操作，下面语句正确的是（）

A.



```GO
i := 1
i++
```



B.



```GO
i := 1
j = i++
```



C.



```GO
i := 1
++i
```



D.



```GO
i := 1
i--
```



参考答案：AD



### 25、**[intermediate]** 关于函数声明，下面语法错误的是（）

A. func f(a, b int) (value int, err error)

B. func f(a int, b int) (value int, err error)

C. func f(a, b int) (value int, error)

D. func f(a int, b int) (int, int, error)



参考答案：C



### 26、**[intermediate]** 如果Add函数的调用代码为：



```GO
func main() {
        var a Integer = 1
        var b Integer = 2
        var i interface{} = &a
        sum := i.(*Integer).Add(b)
        fmt.Println(sum)
}
```



### 27、则Add函数定义正确的是（）

A.



```GO
type Integer int
func (a Integer) Add(b Integer) Integer {
        return a + b
}
```



B.



```GO
type Integer int
func (a Integer) Add(b *Integer) Integer {
        return a + *b
}
```



C.



```GO
type Integer int
func (a *Integer) Add(b Integer) Integer {
        return *a + b
}
```



D.



```GO
type Integer int
func (a *Integer) Add(b *Integer) Integer {
        return *a + *b
}
```



参考答案：AC



### 28、**[intermediate]** 如果Add函数的调用代码为：



```GO
func main() {
        var a Integer = 1
        var b Integer = 2
        var i interface{} = a
        sum := i.(Integer).Add(b)
        fmt.Println(sum)
}
```



### 29、则Add函数定义正确的是（）

A.



```GO
type Integer int
func (a Integer) Add(b Integer) Integer {
        return a + b
}
```



B.



```GO
type Integer int
func (a Integer) Add(b *Integer) Integer {
        return a + *b
}
```



C.



```GO
type Integer int
func (a *Integer) Add(b Integer) Integer {
        return *a + b
}
```



D.



```GO
type Integer int
func (a *Integer) Add(b *Integer) Integer {
        return *a + *b
}
```



参考答案：A



### 30、**[intermediate]** 关于GetPodAction定义，下面赋值正确的是（）



```GO
type Fragment interface {
        Exec(transInfo *TransInfo) error
}
type GetPodAction struct {
}
func (g GetPodAction) Exec(transInfo *TransInfo) error {
        ...
        return nil
}
```



A. var fragment Fragment = new(GetPodAction)

B. var fragment Fragment = GetPodAction

C. var fragment Fragment = &GetPodAction{}

D. var fragment Fragment = GetPodAction{}



参考答案：ACD



### 31、**[intermediate]** 关于GoMock，下面说法正确的是（）

A. GoMock可以对interface打桩

B. GoMock可以对类的成员函数打桩

C. GoMock可以对函数打桩

D. GoMock打桩后的依赖注入可以通过GoStub完成



参考答案：AD



### 32、**[intermediate]** 关于接口，下面说法正确的是（）

A. 只要两个接口拥有相同的方法列表（次序不同不要紧），那么它们就是等价的，可以相互赋值

B. 如果接口A的方法列表是接口B的方法列表的子集，那么接口B可以赋值给接口A

C. 接口查询是否成功，要在运行期才能够确定

D. 接口赋值是否可行，要在运行期才能够确定



参考答案：ABC



### 33、**[primary]** 关于channel，下面语法正确的是（）

A. var ch chan int

B. ch := make(chan int)

C. <- ch

D. ch <-



参考答案：ABC



### 34、**[primary]** 关于同步锁，下面说法正确的是（）

A. 当一个goroutine获得了Mutex后，其他goroutine就只能乖乖的等待，除非该goroutine释放这个Mutex

B. RWMutex在读锁占用的情况下，会阻止写，但不阻止读

C. RWMutex在写锁占用情况下，会阻止任何其他goroutine（无论读和写）进来，整个锁相当于由该goroutine独占

D. Lock()操作需要保证有Unlock()或RUnlock()调用与之对应



参考答案：ABC



### 35、**[intermediate]** golang中大多数数据类型都可以转化为有效的JSON文本，下面几种类型除外（）

A. 指针

B. channel

C. complex

D. 函数



参考答案：BCD



### 36、**[intermediate]** 关于go vendor，下面说法正确的是（）

A. 基本思路是将引用的外部包的源代码放在当前工程的vendor目录下面

B. 编译go代码会优先从vendor目录先寻找依赖包

C. 可以指定引用某个特定版本的外部包

D. 有了vendor目录后，打包当前的工程代码到其他机器的$GOPATH/src下都可以通过编译



参考答案：ABD



### 37、**[primary]** flag是bool型变量，下面if表达式符合编码规范的是（）

A. if flag == 1

B. if flag

C. if flag == false

D. if !flag



参考答案：BD



### 38、**[primary]** value是整型变量，下面if表达式符合编码规范的是（）

A. if value == 0

B. if value

C. if value != 0

D. if !value



参考答案：AC



### 39、**[intermediate]** 关于函数返回值的错误设计，下面说法正确的是（）

A. 如果失败原因只有一个，则返回bool

B. 如果失败原因超过一个，则返回error

C. 如果没有失败原因，则不返回bool或error

D. 如果重试几次可以避免失败，则不要立即返回bool或error



参考答案：ABCD



### 40、**[intermediate]** 关于异常设计，下面说法正确的是（）

A. 在程序开发阶段，坚持速错，让程序异常崩溃

B. 在程序部署后，应恢复异常避免程序终止

C. 一切皆错误，不用进行异常设计

D. 对于不应该出现的分支，使用异常处理



参考答案：ABD



### 41、**[intermediate]** 关于slice或map操作，下面正确的是（）

A.



```GO
var s []int
s = append(s,1)
```



B.



```GO
var m map[string]int
m["one"] = 1
```



C.



```GO
var s []int
s = make([]int, 0)
s = append(s,1)
```



D.



```GO
var m map[string]int
m = make(map[string]int)
m["one"] = 1
```



参考答案：ACD



### 42、**[intermediate]** 关于channel的特性，下面说法正确的是（）

A. 给一个 nil channel 发送数据，造成永远阻塞

B. 从一个 nil channel 接收数据，造成永远阻塞

C. 给一个已经关闭的 channel 发送数据，引起 panic

D. 从一个已经关闭的 channel 接收数据，如果缓冲区中为空，则返回一个零值



参考答案：ABCD



### 43、**[intermediate]** 关于无缓冲和有冲突的channel，下面说法正确的是（）

A. 无缓冲的channel是默认的缓冲为1的channel

B. 无缓冲的channel和有缓冲的channel都是同步的

C. 无缓冲的channel和有缓冲的channel都是非同步的

D. 无缓冲的channel是同步的，而有缓冲的channel是非同步的



参考答案：D



### 44、**[intermediate]** 关于异常的触发，下面说法正确的是（）

A. 空指针解析

B. 下标越界

C. 除数为0

D. 调用panic函数



参考答案：ABCD



### 45、**[intermediate]** 关于cap函数的适用类型，下面说法正确的是（）

A. array

B. slice

C. map

D. channel



参考答案：ABD



### 46、**[intermediate]** 关于beego框架，下面说法正确的是（）

A. beego是一个golang实现的轻量级HTTP框架

B. beego可以通过注释路由、正则路由等多种方式完成url路由注入

C. 可以使用bee new工具生成空工程，然后使用bee run命令自动热编译

D. beego框架只提供了对url路由的处理， 而对于MVC架构中的数据库部分未提供框架支持



参考答案：ABC



### 47、**[intermediate]** 关于goconvey，下面说法正确的是（）

A. goconvey是一个支持golang的单元测试框架

B. goconvey能够自动监控文件修改并启动测试，并可以将测试结果实时输出到web界面

C. goconvey提供了丰富的断言简化测试用例的编写

D. goconvey无法与go test集成



参考答案：ABC



### 48、**[intermediate]** 关于go vet，下面说法正确的是（）

A. go vet是golang自带工具go tool vet的封装

B. 当执行go vet database时，可以对database所在目录下的所有子文件夹进行递归检测

C. go vet可以使用绝对路径、相对路径或相对GOPATH的路径指定待检测的包

D. go vet可以检测出死代码



参考答案：ACD



### 49、**[intermediate]** 关于map，下面说法正确的是（）

A. map反序列化时json.unmarshal的入参必须为map的地址

B. 在函数调用中传递map，则子函数中对map元素的增加不会导致父函数中map的修改

C. 在函数调用中传递map，则子函数中对map元素的修改不会导致父函数中map的修改

D. 不能使用内置函数delete删除map的元素

参考答案：A



### 50、**[intermediate]** 关于GoStub，下面说法正确的是（）

A. GoStub可以对全局变量打桩

B. GoStub可以对函数打桩

C. GoStub可以对类的成员方法打桩

D. GoStub可以打动态桩，比如对一个函数打桩后，多次调用该函数会有不同的行为



参考答案：ABD



### 51、**[primary]** 关于select机制，下面说法正确的是（）

A. select机制用来处理异步IO问题

B. select机制最大的一条限制就是每个case语句里必须是一个IO操作

C. golang在语言级别支持select关键字

D. select关键字的用法与switch语句非常类似，后面要带判断条件



参考答案：ABC



### 52、**[primary]** 关于内存泄露，下面说法正确的是（）

A. golang有自动垃圾回收，不存在内存泄露

B. golang中检测内存泄露主要依靠的是pprof包

C. 内存泄露可以在编译阶段发现

D. 应定期使用浏览器来查看系统的实时内存信息，及时发现内存泄露问题



参考答案：BD



## 填空题



### 1、**[primary]** 声明一个整型变量i__________



参考答案：var i int



### 2、**[primary]** 声明一个含有10个元素的整型数组a__________



参考答案：var a [10]int



### 3、**[primary]** 声明一个整型数组切片s__________



参考答案：var s []int



### 4、**[primary]** 声明一个整型指针变量p__________



参考答案：var p *int



### 5、**[primary]** 声明一个key为字符串型value为整型的map变量m__________



参考答案：var m map[string]int



### 6、**[primary]** 声明一个入参和返回值均为整型的函数变量f__________



参考答案：var f func(a int) int



### 7、**[primary]** 声明一个只用于读取int数据的单向channel变量ch__________



参考答案：var ch <-chan int



### 8、**[primary]** 假设源文件的命名为slice.go，则测试文件的命名为__________



参考答案：slice_test.go



### 9、**[primary]** go test要求测试函数的前缀必须命名为__________



参考答案：Test



### 10、**[intermediate]** 下面的程序的运行结果是__________



```GO
for i := 0; i < 5; i++ {
        defer fmt.Printf("%d ", i)
}
```



参考答案：4 3 2 1 0



### 11、**[intermediate]** 下面的程序的运行结果是__________



```GO
func main() {
        x := 1
        {
            x := 2
            fmt.Print(x)
        }
        fmt.Println(x)
}
```



参考答案：21



### 12、**[intermediate]** 下面的程序的运行结果是__________



```GO
func main() {
        strs := []string{"one", "two", "three"}

        for _, s := range strs {
            go func() {
                time.Sleep(1 * time.Second)
                fmt.Printf("%s ", s)
            }()
        }
        time.Sleep(3 * time.Second)
}
```



参考答案：three three three



### 13、**[intermediate]** 下面的程序的运行结果是__________



```GO
func main() {
        x := []string{"a", "b", "c"}
        for v := range x {
            fmt.Print(v)
        }
}
```



参考答案：012



### 14、**[intermediate]** 下面的程序的运行结果是__________



```GO
func main() {
        x := []string{"a", "b", "c"}
        for _, v := range x {
            fmt.Print(v)
        }
}
```



参考答案：abc



### 15、**[primary]** 下面的程序的运行结果是__________



```GO
func main() {
       i := 1
       j := 2
       i, j = j, i
       fmt.Printf("%d%d\n", i, j)
}
```



参考答案：21



### 16、**[primary]** 下面的程序的运行结果是__________



```GO
func incr(p *int) int {
        *p++
        return *p
}
func main() {
        v := 1
        incr(&v)
        fmt.Println(v)
}
```



参考答案：2



### 17、**[primary]** 启动一个goroutine的关键字是__________



参考答案：go



### 18、**[intermediate]** 下面的程序的运行结果是__________



```GO
type Slice []int
func NewSlice() Slice {
         return make(Slice, 0)
}
func (s* Slice) Add(elem int) *Slice {
         *s = append(*s, elem)
         fmt.Print(elem)
         return s
}
func main() {
         s := NewSlice()
         defer s.Add(1).Add(2)
         s.Add(3)
}
```



参考答案：132



## 判断题



### 1、**[primary]** 数组是一个值类型（）



参考答案：T



### 2、**[primary]** 使用map不需要引入任何库（）



参考答案：T



### 3、**[intermediate]** 内置函数delete可以删除数组切片内的元素（）



参考答案：F



### 4、**[primary]** 指针是基础类型（）



参考答案：F



### 5、**[primary]** interface{}是可以指向任意对象的Any类型（）



参考答案：T



### 6、**[intermediate]** 下面关于文件操作的代码可能触发异常（）



```GO
file, err := os.Open("test.go")
defer file.Close()
if err != nil {
        fmt.Println("open file failed:", err)
        return
}
...
```



参考答案：T



### 7、**[primary]** Golang不支持自动垃圾回收（）



参考答案：F



### 8、**[primary]** Golang支持反射，反射最常见的使用场景是做对象的序列化（）



参考答案：T



### 9、**[primary]** Golang可以复用C/C++的模块，这个功能叫Cgo（）



参考答案：F



### 10、**[primary]** 下面代码中两个斜点之间的代码，比如`json:"x"`，作用是X字段在从结构体实例编码到JSON数据格式的时候，使用x作为名字，这可以看作是一种重命名的方式（）



```GO
type Position struct {
        X int `json:"x"`
        Y int `json:"y"`
        Z int `json:"z"`
}
```



参考答案：T



### 11、**[primary]** 通过成员变量或函数首字母的大小写来决定其作用域（）



参考答案：T



### 12、**[primary]** 对于常量定义zero(const zero = 0.0)，zero是浮点型常量（）



参考答案：F



### 13、**[primary]** 对变量x的取反操作是~x（）



参考答案：F



### 14、**[primary]** 下面的程序的运行结果是xello（）



```GO
func main() {
        str := "hello"
        str[0] = 'x'
        fmt.Println(str)
}
```



参考答案：F



### 15、**[primary]** golang支持goto语句（）



参考答案：T



### 16、**[primary]** 下面代码中的指针p为野指针，因为返回的栈内存在函数结束时会被释放（）



```GO
type TimesMatcher struct {
        base int
}
func NewTimesMatcher(base int) *TimesMatcher{
        return &TimesMatcher{base:base}
}
func main() {
        p := NewTimesMatcher(3)
        ...
}
```



参考答案：F



### 17、**[primary]** 匿名函数可以直接赋值给一个变量或者直接执行（）



参考答案：T



### 18、**[primary]** 如果调用方调用了一个具有多返回值的方法，但是却不想关心其中的某个返回值，可以简单地用一个下划线“_”来跳过这个返回值，该下划线对应的变量叫匿名变量（）



参考答案：T



### 19、**[primary]** 在函数的多返回值中，如果有error或bool类型，则一般放在最后一个（）



参考答案：T



### 20、**[primary]** 错误是业务过程的一部分，而异常不是（）



参考答案：T



### 21、**[primary]** 函数执行时，如果由于panic导致了异常，则延迟函数不会执行（）



参考答案：F



### 22、**[intermediate]** 当程序运行时，如果遇到引用空指针、下标越界或显式调用panic函数等情况，则先触发panic函数的执行，然后调用延迟函数。调用者继续传递panic，因此该过程一直在调用栈中重复发生：函数停止执行，调用延迟执行函数。如果一路在延迟函数中没有recover函数的调用，则会到达该携程的起点，该携程结束，然后终止其他所有携程，其他携程的终止过程也是重复发生：函数停止执行，调用延迟执行函数（）



参考答案：F



### 23、**[primary]** 同级文件的包名不允许有多个（）



参考答案：T



### 24、**[intermediate]** 可以给任意类型添加相应的方法（）



参考答案：F



### 25、**[primary]** golang虽然没有显式的提供继承语法，但是通过匿名组合实现了继承（）



参考答案：T



### 26、**[primary]** 使用for range迭代map时每次迭代的顺序可能不一样，因为map的迭代是随机的（）



参考答案：T



### 27、**[primary]** switch后面可以不跟表达式（）



参考答案：T



### 28、**[intermediate]** 结构体在序列化时非导出变量（以小写字母开头的变量名）不会被encode，因此在decode时这些非导出变量的值为其类型的零值（）



参考答案：T



### 29、**[primary]** golang中没有构造函数的概念，对象的创建通常交由一个全局的创建函数来完成，以NewXXX来命名（）



参考答案：T



### 30、**[intermediate]** 当函数deferDemo返回失败时，并不能destroy已create成功的资源（）



```GO
func deferDemo() error {
        err := createResource1()
        if err != nil {
            return ERR_CREATE_RESOURCE1_FAILED
        }
        defer func() {
            if err != nil {
                destroyResource1()
            }
        }()

        err = createResource2()
        if err != nil {
            return ERR_CREATE_RESOURCE2_FAILED
        }
        defer func() {
            if err != nil {
                destroyResource2()
            }
        }()

        err = createResource3()
        if err != nil {
            return ERR_CREATE_RESOURCE3_FAILED
        }
        return nil
}
```



参考答案：F



### 31、**[intermediate]** channel本身必然是同时支持读写的，所以不存在单向channel（）



参考答案：F



### 32、**[primary]** import后面的最后一个元素是包名（）



参考答案：F
