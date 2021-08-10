# 什么是协程？
Go协程（Goroutine）是与其他函数或方法同时运行的函数或方法。可以认为Go协程是轻量级的线程。与创建线程相比，创建Go协程的成本很小。因此在Go中同时运行上千个协程是很常见的。

# Go协程对比线程的优点
与线程相比，Go协程的开销非常小。Go协程的堆栈大小只有几kb，它可以根据应用程序的需要而增长和缩小，而线程必须指定堆栈的大小，并且堆栈的大小是固定的。<br>
Go协程被多路复用到较少的OS线程。在一个程序中数千个Go协程可能只运行在一个线程中。如果该线程中的任何一个Go协程阻塞（比如等待用户输入），那么Go会创建一个新的OS线程并将其余的Go协程移动到这个新的OS线程。所有这些操作都是 runtime 来完成的，而我们程序员不必关心这些复杂的细节，只需要利用 Go 提供的简洁的 API 来处理并发就可以了。<br>
Go 协程之间通过信道（channel）进行通信。信道可以防止多个协程访问共享内存时发生竟险（race condition）。信道可以想象成多个协程之间通信的管道。我们将在下一篇教程中介绍信道。<br>

# 如何创建一个协程？
在函数或方法调用之前加上关键字 go，这样便开启了一个并发的Go协程。

让我们创建一个协程：
```go
package main
import (  
    "fmt"
)
func hello() {  
    fmt.Println("Hello world goroutine")
}
func main() {  
    go hello()
    fmt.Println("main function")
}
````
第11行，go hello() 开启了一个新的协程。现在 hello() 函数将和 main() 函数一起运行。main 函数在单独的协程中运行，这个协程称为主协程。

运行这个程序，你将得到一个惊喜。程序仅输出了一行文本： main function。

**我们创建的协程发生了什么？我们需要了解Go协程的两个属性，以了解为什么发生这种情况。**

当创建一个Go协程时，创建这个Go协程的语句立即返回。与函数不同，程序流程不会等待Go协程结束再继续执行。程序流程在开启Go协程后立即返回并开始执行下一行代码，忽略Go协程的任何返回值。<br>
在主协程存在时才能运行其他协程，主协程终止则程序终止，其他协程也将终止。<br>
我想你已经知道了为什么我们的协程为什么没有运行。在11行调用 go hello()后，程序的流程直接调转到下一条语句执行，并没有等待 hello 协程退出，然后打印 main function。<br>

接着主协程结束运行，不会再执行任何代码，因此 hello 协程没有得到运行的机会。

让我们修复这个问题：
```go
package main
import (  
    "fmt"
    "time"
)
func hello() {  
    fmt.Println("Hello world goroutine")
}
func main() {  
    go hello()
    time.Sleep(1 * time.Second)
    fmt.Println("main function")
}
```
上面的程序中，第13行，我们调用 time 包的 Sleep 函数来使调用该函数所在的协程休眠。在这里是让主协程休眠1秒钟。现在调用 go hello() 有了足够的时间得以在主协程退出之前执行。该程序首先打印 Hello world goroutine，等待1秒钟之后打印 main function。

在主协程中使用 Sleep 函数等待其他协程结束的方法是不正规的，我们用在这里只是为了说明Go协程是如何工作的。信道可以用于阻塞主协程，直到其他协程执行完毕。我们将在下一篇教程中讨论信道。

# 开启多个协程
让我们写一个程序开启多个协程来更好的理解协程。
```go
package main
import (  
    "fmt"
    "time"
)
func numbers() {  
    for i := 1; i <= 5; i++ {
        time.Sleep(250 * time.Millisecond)
        fmt.Printf("%d ", i)
    }
}
func alphabets() {  
    for i := 'a'; i <= 'e'; i++ {
        time.Sleep(400 * time.Millisecond)
        fmt.Printf("%c ", i)
    }
}
func main() {  
    go numbers()
    go alphabets()
    time.Sleep(3000 * time.Millisecond)
    fmt.Println("main terminated")
}
```
上面的程序在第21和22行开启了两个协程。现在这两个协程同时执行。numbers 协程最初睡眠 250 毫秒，然后打印 1，接着再次睡眠然后打印2，以此类推，直到打印到 5。类似地，alphabets 协程打印从 a 到 e 的字母，每个字母之间相隔 400 毫秒。主协程开启 numbers 和 alphabets 协程，等待 3000 毫秒，最后终止。

程序的输出为：

    1 a 2 3 b 4 c 5 d e main terminated
    
下面的图片描述了这个程序是如何工作的，请在新的标签中打开图像以获得更好的效果：

![image](https://user-images.githubusercontent.com/87457873/128817121-d9d06e47-3232-4187-a2b7-a595085e2a12.png)


上图中，蓝色的线框表示 numbers 协程，栗色的线框表示 alphabets 协程。绿色的线框表示主协程。黑色的线框合并了上述三个协程，向我们展示了该程序的工作原理。每个框顶部的 0ms，250 ms 的字符串表示以毫秒为单位的时间，在每个框底部的 1，2，3 表示输出。

蓝色的线框告诉我们在 250ms 的时候打印了1，在 500ms 的时候打印了2，以此类推。因此最后一个线框底部的输出：1 a 2 3 b 4 c 5 d e main terminated 也是整个程序的输出。上面的图像是很好理解的，您将能够了解该程序的工作原理。
