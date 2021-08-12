>摘要：文将详细介绍 Golang 的语言特点以及它的优缺点和适用场景，带着上述几个疑问，为读者分析 Go 语言的各个方面，以帮助初入 IT 行业的程序员以及对 Go 感兴趣的开发者进一步了解这个热门语言。

本文分享自华为云社区《大红大紫的 Golang 真的是后端开发中的万能药吗？》，原文作者：Marvin Zhang 。

# 前言
城外的人想进去，城里的人想出来。– 钱钟书《围城》

随着容器编排（Container Orchestration）、微服务（Micro Services）、云技术（Cloud Technology）等在 IT 行业不断盛行，2009 年诞生于 Google 的 Golang（Go 语言，简称 Go）越来越受到软件工程师的欢迎和追捧，成为如今炙手可热的后端编程语言。在用 Golang 开发的软件项目列表中，有 Docker（容器技术）、Kubernetes（容器编排）这样的颠覆整个 IT 行业的明星级产品，也有像 Prometheus（监控系统）、Etcd（分布式存储）、InfluxDB（时序数据库）这样的强大实用的知名项目。当然，Go 语言的应用领域也绝不局限于容器和分布式系统。如今很多大型互联网企业在大量使用 Golang 构建后端 Web 应用，例如今日头条、京东、七牛云等；长期被 Python 统治的框架爬虫领域也因为简单而易用的爬虫框架 Colly 的崛起而不断受到 Golang 的挑战。Golang 已经成为了如今大多数软件工程师最想学习的编程语言。下图是 HackerRank 在 2020 年调查程序员技能的相关结果。

![image](https://user-images.githubusercontent.com/87457873/129165592-63a47df3-3ee1-4e0e-9175-1f78069f36c6.png)


那么，Go 语言真的是后端开发人员的救命良药呢？它是否能够有效提高程序员们的技术实力和开发效率，从而帮助他们在职场上更进一步呢？Go 语言真的值得我们花大量时间深入学习么？本文将详细介绍 Golang 的语言特点以及它的优缺点和适用场景，带着上述几个疑问，为读者分析 Go 语言的各个方面，以帮助初入 IT 行业的程序员以及对 Go 感兴趣的开发者进一步了解这个热门语言。

# Golang 简介

![image](https://user-images.githubusercontent.com/87457873/129165623-b77f0780-74d9-4dd0-8e2d-966e697e761e.png)

Golang 诞生于互联网巨头 Google，而这并不是一个巧合。我们都知道，Google 有一个 20% 做业余项目（Side Project）的企业文化，允许工程师们能够在轻松的环境下创造一些具有颠覆性创新的产品。而 Golang 也正是在这 20% 时间中不断孵化出来。Go 语言的创始者也是 IT 界内大名鼎鼎的行业领袖，包括 Unix 核心团队成员 Rob Pike、C 语言作者 Ken Thompson、V8 引擎核心贡献者 Robert Griesemer。Go 语言被大众所熟知还是源于容器技术 Docker 在 2014 年被开源后的爆发式发展。之后，Go 语言因为其简单的语法以及迅猛的编译速度受到大量开发者的追捧，也诞生了很多优秀的项目，例如 Kubernetes。

Go 语言相对于其他传统热门编程语言来说，有很多优点，特别是其高效编译速度和天然并发特性，让其成为快速开发分布式应用的首选语言。Go 语言是静态类型语言，也就是说 Go 语言跟 Java、C# 一样需要编译，而且有完备的类型系统，可以有效减少因类型不一致导致的代码质量问题。因此，Go 语言非常适合构建对稳定性和灵活性均有要求的大型 IT 系统，这也是很多大型互联网公司用 Golang 重构老代码的重要原因：传统的静态 OOP 语言（例如 Java、C#）稳定性高但缺乏灵活性；而动态语言（例如 PHP、Python、Ruby、Node.js）灵活性强但缺乏稳定性。因此，“熊掌和鱼兼得” 的 Golang，受到开发者们的追捧是自然而然的事情，毕竟，“天下苦 Java/PHP/Python/Ruby 们久矣“。

不过，Go 语言并不是没有缺点。用辩证法的思维方式可以推测，Golang 的一些突出特性将成为它的双刃剑。例如，Golang 语法简单的优势特点将限制它处理复杂问题的能力。尤其是 Go 语言缺乏泛型（Generics）的问题，导致它构建通用框架的复杂度大增。虽然这个突出问题在 2.0 版本很可能会有效解决，但这也反映出来明星编程语言也会有缺点。当然，Go 的缺点还不止于此，Go 语言使用者还会吐槽其啰嗦的错误处理方式（Error Handling）、缺少严格约束的鸭子类型（Duck Typing）、日期格式问题等。下面，我们将从 Golang 语言特点开始，由浅入深多维度深入分析 Golang 的优缺点以及项目适用场景。

# 语言特点

## 简洁的语法特征

Go 语言的语法非常简单，至少在变量声明、结构体声明、函数定义等方面显得非常简洁。

变量的声明不像 Java 或 C 那样啰嗦，在 Golang 中可以用 := 这个语法来声明新变量。例如下面这个例子，当你直接使用 := 来定义变量时，Go 会自动将赋值对象的类型声明为赋值来源的类型，这节省了大量的代码。

```go
func main() {
  valInt := 1  // 自动推断 int 类型
  valStr := "hello"  // 自动推断为 string 类型
  valBool := false  // 自动推断为 bool 类型
}
```

Golang 还有很多帮你节省代码的地方。你可以发现 Go 中不会强制要求用 new 这个关键词来生成某个类（Class）的新实例（Instance）。而且，对于公共和私有属性（变量和方法）的约定不再使用传统的 public 和 private 关键词，而是直接用属性变量首字母的大小写来区分。下面一些例子可以帮助读者理解这些特点。

```go
// 定义一个 struct 类
type SomeClass struct {
  PublicVariable string  // 公共变量
  privateVariable string  // 私有变量
}

// 公共方法
func (c *SomeClass) PublicMethod() (result string) {
  return "This can be called by external modules"
}

// 私有方法
func (c *SomeClass) privateMethod() (result string) {
  return "This can only be called in SomeClass"
}

func main() {
  // 生成实例
  someInstance := SomeClass{
    PublicVariable: "hello",
    privateVariable: "world",
  }
}
```

如果你用 Java 来实现上述这个例子，可能会看到冗长的 .java 类文件，例如这样。

```go
// SomeClass.java
public SomeClass {
  public String PublicVariable;  // 公共变量
  private String privateVariable;  // 私有变量
 
  // 构造函数
  public SomeClass(String val1, String val2) {
    this.PublicVariable = val1;
    this.privateVariable = val2;
  }
 
  // 公共方法
  public String PublicMethod() {
    return "This can be called by external modules";
  }
 
  // 私有方法
  public String privateMethod() {
    return "This can only be called in SomeClass";
  }
}

...
 
// Application.java
public Application {
  public static void main(String[] args) {
    // 生成实例
    SomeClass someInstance = new SomeClass("hello", "world");
  }
}
```

可以看到，在 Java 代码中除了容易看花眼的多层花括号以外，还充斥着大量的 public、private、static、this 等修饰用的关键词，显得异常啰嗦；而 Golang 代码中则靠简单的约定，例如首字母大小写，避免了很多重复性的修饰词。当然，Java 和 Go 在类型系统上还是有一些区别的，这也导致 Go 在处理复杂问题显得有些力不从心，这是后话，后面再讨论。总之，结论就是 Go 的语法在静态类型编程语言中非常简洁。

## 内置并发编程

Go 语言之所以成为分布式应用的首选，除了它性能强大以外，其最主要的原因就是它天然的并发编程。这个并发编程特性主要来自于 Golang 中的协程（Goroutine）和通道（Channel）。下面是使用协程的一个例子。

```go
func asyncTask() {
  fmt.Printf("This is an asynchronized task")
}

func syncTask() {
  fmt.Printf("This is a synchronized task")
}

func main() {
  go asyncTask()  // 异步执行，不阻塞
  syncTask()  // 同步执行，阻塞
  go asyncTask()  // 等待前面 syncTask 完成之后，再异步执行，不阻塞
}
```

可以看到，关键词 go 加函数调用可以让其作为一个异步函数执行，不会阻塞后面的代码。而如果不加 go 关键词，则会被当成是同步代码执行。如果读者熟悉 JavaScript 中的 async/await、Promise 语法，甚至是 Java、Python 中的多线程异步编程，你会发现它们跟 Go 异步编程的简单程度不是一个量级的！

异步函数，也就是协程之间的通信可以用 Go 语言特有的通道来实现。下面是关于通道的一个例子。

```go
func longTask(signal chan int) {
  // 不带参数的 for
  // 相当于 while 循环
  for {
    // 接收 signal 通道传值
    v := <- signal
 
    // 如果接收值为 1，停止循环
    if v == 1 {
      break
    }
 
    time.Sleep(1 * Second)
  }
}

func main() {
  // 声明通道
  sig := make(chan int)
 
  // 异步调用 longTask
  go longTask(sig)
 
  // 等待 1 秒钟
  time.Sleep(1 * time.Second)
 
  // 向通道 sig 传值
  sig <- 1
 
  // 然后 longTask 会接收 sig 传值，终止循环
}
```

## 面向接口编程

Go 语言不是严格的面向对象编程（OOP），它采用的是面向接口编程（IOP），是相对于 OOP 更先进的编程模式。作为 OOP 体系的一部分，IOP 更加强调规则和约束，以及接口类型方法的约定，从而让开发人员尽可能的关注更抽象的程序逻辑，而不是在更细节的实现方式上浪费时间。很多大型项目采用的都是 IOP 的编程模式。如果想了解更多面向接口编程，请查看 “码之道” 个人技术博客的往期文章《为什么说 TypeScript 是开发大型前端项目的必备语言》，其中有关于面向接口编程的详细讲解。

Go 语言跟 TypeScript 一样，也是采用鸭子类型的方式来校验接口继承。下面这个例子可以描述 Go 语言的鸭子类型特性。

```go
// 定义 Animal 接口
interface Animal {
  Eat()  // 声明 Eat 方法
  Move()  // 声明 Move 方法
}

// ==== 定义 Dog Start ====
// 定义 Dog 类
type Dog struct {
}

// 实现 Eat 方法
func (d *Dog) Eat() {
  fmt.Printf("Eating bones")
}

// 实现 Move 方法
func (d *Dog) Move() {
  fmt.Printf("Moving with four legs")
}
// ==== 定义 Dog End ====

// ==== 定义 Human Start ====
// 定义 Human 类
type Human struct {
}

// 实现 Eat 方法
func (h *Human) Eat() {
  fmt.Printf("Eating rice")
}

// 实现 Move 方法
func (h *Human) Move() {
  fmt.Printf("Moving with two legs")
}
// ==== 定义 Human End ====
```

可以看到，虽然 Go 语言可以定义接口，但跟 Java 不同的是，Go 语言中没有显示声明接口实现（Implementation）的关键词修饰语法。在 Go 语言中，如果要继承一个接口，你只需要在结构体中实现该接口声明的所有方法。这样，对于 Go 编译器来说你定义的类就相当于继承了该接口。在这个例子中，我们规定，只要既能吃（Eat）又能活动（Move）的东西就是动物（Animal）。而狗（Dog）和人（Human）恰巧都可以吃和动，因此它们都被算作动物。这种依靠实现方法匹配度的继承方式，就是鸭子类型：如果一个动物看起来像鸭子，叫起来也像鸭子，那它一定是鸭子。这种鸭子类型相对于传统 OOP 编程语言显得更灵活。但是，后面我们会讨论到，这种编程方式会带来一些麻烦。

## 错误处理

Go 语言的错误处理是臭名昭著的啰嗦。这里先给一个简单例子。

```go
package main

import "fmt"

func isValid(text string) (valid bool, err error){
  if text == "" {
    return false, error("text cannot be empty")
  }
  return text == "valid text", nil
}

func validateForm(form map[string]string) (res bool, err error) {
  for _, text := range form {
    valid, err := isValid(text)
    if err != nil {
      return false, err
    }
    if !valid {
      return false, nil
    }
  }
  return true, nil
}

func submitForm(form map[string]string) (err error) {
  if res, err := validateForm(form); err != nil || !res {
    return error("submit error")
  }
  fmt.Printf("submitted")
  return nil
}

func main() {
  form := map[string]string{
    "field1": "",
    "field2": "invalid text",
    "field2": "valid text",
  }
  if err := submitForm(form); err != nil {
    panic(err)
  }
}
```

虽然上面整个代码是虚构的，但可以从中看出，Go 代码中充斥着 if err := …; err != nil { … } 之类的错误判断语句。这是因为 Go 语言要求开发者自己管理错误，也就是在函数中的错误需要显式抛出来，否则 Go 程序不会做任何错误处理。因为 Go 没有传统编程语言的 try/catch 针对错误处理的语法，所以在错误管理上缺少灵活度，导致了 “err 满天飞” 的局面。

不过，辩证法则告诉我们，这种做法也是有好处的。第一，它强制要求 Go 语言开发者从代码层面来规范错误的管理方式，这驱使开发者写出更健壮的代码；第二，这种显式返回错误的方式避免了 “try/catch 一把梭”，因为这种 “一时爽” 的做法很可能导致 Bug 无法准确定位，从而产生很多不可预测的问题；第三，由于没有 try/catch 的括号或额外的代码块，Go 程序代码整体看起来更清爽，可读性较强。

## 其他
Go 语言肯定还有很多其他特性，但笔者认为以上的特性是 Go 语言中比较有特色的，是区分度比较强的特性。Go 语言其他一些特性还包括但不限于如下内容。

* 编译迅速
* 跨平台
* defer 延迟执行
* select/case 通道选择
* 直接编译成可执行程序
* 非常规依赖管理（可以直接引用 Github 仓库作为依赖，例如 import "github.com/crawlab-team/go-trace"）
* 非常规日期格式（格式为 “2006-01-02 15:04:05”，你没看错，据说这就是 Golang 的创始时间！）

# 优缺点概述

前面介绍了 Go 的很多语言特性，想必读者已经对 Golang 有了一些基本的了解。其中的一些语言特性也暗示了它相对于其他编程语言的优缺点。Go 语言虽然现在很火，在称赞并拥抱 Golang 的同时，不得不了解它的一些缺点。

这里笔者不打算长篇大论的解析 Go 语言的优劣，而是将其中相关的一些事实列举出来，读者可以自行判断。以下是笔者总结的 Golang 语言特性的不完整优缺点对比列表。

![image](https://user-images.githubusercontent.com/87457873/129166103-8e7da195-20f6-4def-937e-d5a33f8bc215.png)

其实，每一个特性在某种情境下都有其相应的优势和劣势，不能一概而论。就像 Go 语言采用的静态类型和面向接口编程，既不缺少类型约束，也不像严格 OOP 那样冗长繁杂，是介于动态语言和传统静态类型 OOP 语言之间的现代编程语言。这个定位在提升 Golang 开发效率的同时，也阉割了不少必要 OOP 语法特性，从而缺乏快速构建通用工程框架的能力（这里不是说 Go 无法构建通用框架，而是它没有 Java、C# 这么容易）。另外，Go 语言 “奇葩” 的错误处理规范，让 Go 开发者们又爱又恨：可以开发出更健壮的应用，但同时也牺牲了一部分代码的简洁性。要知道，Go 语言的设计理念是为了 “大道至简”，因此才会在追求高性能的同时设计得尽可能简单。

无可否认的是，Go 语言内置的并发支持是非常近年来非常创新的特性，这也是它被分布式系统广泛采用的重要原因。同时，它相对于动辄编译十几分钟的 Java 来说是非常快的。此外，Go 语言没有因为语法简单而牺牲了稳定性；相反，它从简单的约束规范了整个 Go 项目代码风格。因此，**“快”（Fast）、“简”（Concise）、“稳”（Robust）**是 Go 语言的设计目的。我们在对学习 Golang 的过程中不能无脑的接纳它的一切，而是应该根据它自身的特性判断在实际项目应用中的情况。

# 适用场景

经过前文关于 Golang 各个维度的讨论，我们可以得出结论：Go 语言并不是后端开发的万能药。在实际开发工作中，开发者应该避免在任何情况下无脑使用 Golang 作为后端开发语言。相反，工程师在决定技术选型之前应该全面了解候选技术（语言、框架或架构）的方方面面，包括候选技术与业务需求的切合度，与开发团队的融合度，以及其学习、开发、时间成本等因素。笔者在学习了包括前后端的一些编程语言之后，发现它们各自有各自的优势，也有相应的劣势。如果一门编程语言能广为人知，那它绝对不会是一门糟糕语言。因此，笔者不会断言 “XXX 是世界上最好的语言“，而是给读者分享个人关于特定应用场景下技术选型的思路。当然，本文是针对 Go 语言的技术文，接下来笔者将分享一下个人认为 Golang 最适合的应用场景。

## 分布式应用

Golang 是非常适合在分布式应用场景下开发的。分布式应用的主要目的是尽可能多的利用计算资源和网络带宽，以求最大化系统的整体性能和效率，其中重要的需求功能就是并发（Concurrency）。而 Go 是支持高并发和异步编程方面的佼佼者。

前面已经提到，Go 语言内置了协程（Goroutine）和通道（Channel）两大并发特性，这使后端开发者进行异步编程变得非常容易。Golang 中还内置了sync 库，包含 Mutex（互斥锁）、WaitGroup（等待组）、Pool（临时对象池）等接口，帮助开发者在并发编程中能更安全的掌控 Go 程序的并发行为。Golang 还有很多分布式应用开发工具，例如分布式储存系统（Etcd、SeaweedFS）、RPC 库（gRPC、Thrift）、主流数据库 SDK（mongo-driver、gnorm、redigo）等。这些都可以帮助开发者有效的构建分布式应用。

## 网络爬虫

稍微了解网络爬虫的开发者应该会听说过 Scrapy，再不济也是 Python。市面上关于 Python 网络爬虫的技术书籍数不胜数，例如崔庆才的《Python 3 网络开发实战》和韦世东的《Python 3 网络爬虫宝典》。用 Python 编写的高性能爬虫框架 Scrapy，自发布以来一直是爬虫工程师的首选。

不过，由于近期 Go 语言的迅速发展，越来越多的爬虫工程师注意到用 Golang 开发网路爬虫的巨大优势。其中，用 Go 语言编写的 Colly 爬虫框架，如今在 Github 上已经有 13k+ 标星。其简洁的 API 以及高效的采集速度，吸引了很多爬虫工程师，占据了爬虫界一哥 Scrapy 的部分份额。前面已经提到，Go 语言内置的并发特性让严重依赖网络带宽的爬虫程序更加高效，很大的提高了数据采集效率。另外，Go 语言作为静态语言，相对于动态语言 Python 来说有更好的约束下，因此健壮性和稳定性都更好。

## 后端 API

Golang 有很多优秀的后端框架，它们大部分都非常完备的支持了现代后端系统的各种功能需求：RESTful API、路由、中间件、配置、鉴权等模块。而且用 Golang 写的后端应用性能很高，通常有非常快的响应速度。笔者曾经在开源爬虫管理平台 Crawlab 中用 Golang 重构了 Python 的后端 API，响应速度从之前的几百毫秒优化到了几十毫秒甚至是几毫秒，用实践证明 Go 语言在后端性能方面全面碾压动态语言。Go 语言中比较知名的后端框架有 Gin、Beego、Echo、Iris。

当然，这里并不是说用 Golang 写后端就完全是一个正确的选择。笔者在工作中会用到 Java 和 C#，用了各自的主流框架（SpringBoot 和 .Net Core）之后，发现这两门传统 OOP 语言虽然语法啰嗦，但它们的语法特性很丰富，特别是泛型，能够轻松应对一些逻辑复杂、重复性高的业务需求。因此，笔者认为在考虑用 Go 来编写后端 API 时候，可以提前调研一下 Java 或 C#，它们在写后端业务功能方面做得非常棒。

# 总结

本篇文章从 Go 语言的主要语法特性入手，循序渐进分析了 Go 语言作为后端编程语言的优点和缺点，以及其在实际软件项目开发中的试用场景。笔者认为 Go 语言与其他语言的主要区别在于语法简洁、天然支持并发、面向接口编程、错误处理等方面，并且对各个语言特性在正反两方面进行了分析。最后，笔者根据之前的分析内容，得出了 Go 语言作为后端开发编程语言的适用场景，也就是分布式应用、网络爬虫以及后端API。

当然，Go 语言的实际应用领域还不限于此。实际上，不少知名数据库都是用 Golang 开发的，例如时序数据库 Prometheus 和 InfluxDB、以及有 NewSQL 之称的 TiDB。此外，在机器学习方面，Go 语言也有一定的优势，只是目前来说，Google 因为 Swift 跟 TensorFlow 的意向合作，似乎还没有大力推广 Go 在机器学习方面的应用，不过一些潜在的开源项目已经涌现出来，例如 GoLearn、GoML、Gorgonia 等。

在理解 Go 语言的优势和适用场景的同时，我们必须意识到 Go 语言并不是全能的。它相较于其他一些主流框架来说也有一些缺点。开发者在准备采用 Go 作为实际工作开发语言的时候，需要全面了解其语言特性，从而做出最合理的技术选型。就像打网球一样，不仅需要掌握正反手，还要会发球、高压球、截击球等技术动作，这样才能把网球打好。


