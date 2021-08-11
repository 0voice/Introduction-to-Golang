# 👏👏👏最全空降golang资料补给包（满血战斗），包含文章，书籍，作者论文，理论分析，开源框架，云原生，大佬视频，大厂实战分享ppt

<div  align=center>
<img width="70%" height="70%" src="https://user-images.githubusercontent.com/87457873/128655088-7e2704a7-ce37-4e78-9b9c-a8865597f364.png"/>
  
## —— 未来服务器端编程语言
  
</div>

## 📥 [源码下载](https://github.com/0voice/Introduction-to-Golang/blob/main/%E6%BA%90%E7%A0%81/go1.16.7.src.tar.gz)

Go官网下载地址：https://golang.org/dl/

Go官方镜像站（推荐）：https://golang.google.cn/dl/

## 🏃‍♂ [开启Go语言学习之旅，从"Hello World"开始！](https://github.com/0voice/Introduction-to-Golang/blob/main/%E6%90%AD%E5%BB%BAGo%E8%AF%AD%E8%A8%80%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/%E4%BB%8E%E9%9B%B6%E5%BC%80%E5%A7%8B%E6%90%AD%E5%BB%BAGo%E8%AF%AD%E8%A8%80%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83.md)

<div  align=center>
  
![image](https://user-images.githubusercontent.com/87457873/128685884-d52fd32f-ed6e-4b7b-b9ef-a78bef3354a4.png)

</div>

### Step 1：了解源代码目录结构
  
<div  align=center>

![image](https://user-images.githubusercontent.com/87457873/128687226-8ac5bb7c-f09b-4cb3-afc0-cf18bff9d15a.png)
  
</div>

<br>


* |– AUTHORS — 文件，官方 Go语言作者列表
* |– CONTRIBUTORS — 文件，第三方贡献者列表
* |– LICENSE — 文件，Go语言发布授权协议
* |– PATENTS — 文件，专利
* |– README — 文件，README文件，大家懂的。提一下，经常有人说：Go官网打不开啊，怎么办？其实，在README中说到了这个。该文件还提到，如果通过二进制安装，需要设置GOROOT环境变量；如果你将Go放在了/usr/local/go中，则可以不设置该环境变量（Windows下是C:\go）。当然，建议不管什么时候都设置GOROOT。另外，确保$GOROOT/bin在PATH目录中。
* |– VERSION — 文件，当前Go版本
* |– api — 目录，包含所有API列表，方便IDE使用
* |– doc — 目录，Go语言的各种文档，官网上有的，这里基本会有，这也就是为什么说可以本地搭建”官网”。这里面有不少其他资源，比如gopher图标之类的。
* |– favicon.ico — 文件，官网logo
* |– include — 目录，Go 基本工具依赖的库的头文件
* |– lib — 目录，文档模板
* |– misc — 目录，其他的一些工具，相当于大杂烩，大部分是各种编辑器的Go语言支持，还有cgo的例子等
* |– robots.txt — 文件，搜索引擎robots文件
* |– src — 目录，Go语言源码：基本工具（编译器等）、标准库
* |– test — 目录，包含很多测试程序（并非_test.go方式的单元测试，而是包含main包的测试），包括一些fixbug测试。可以通过这个学到一些特性的使用。

### Step 2：Golang开发后台掌握哪些知识点？

这里我给大家整理归纳为四大块，分别是**语法**、**中间件**、**后端开发**、**云原生**。

我们通过这个四个板块的学习，逐步进阶成一个可以从事后端服务器开发的工程师。

<div  align=center>
  
<img width="70%" height="70%" src="https://user-images.githubusercontent.com/87457873/128867223-af882655-eaa7-4523-b737-b3de4c4f5b56.png"/>

</div>

下面我们简单介绍中间件和云原生：

#### 中间件

MySQL、Redis、MongoDB、Kafka这些常见的中间件，这里我们不做赘述。我们着重简述下Gin、etcd、ElasticSearch、gRPC。

* **Gin**<br>
Gin是一个用Go (Golang)编写的HTTP web框架。它具有一个类似martinii的API，性能要好得多——快了40倍。<br>
官方Github项目：https://github.com/gin-gonic/gin

* **etcd**<br>
Etcd是一种强一致性的分布式键值存储，它提供了一种可靠的方法来存储需要被分布式系统或机器集群访问的数据。它可以在网络分区期间优雅地处理leader选举，并且可以容忍机器故障，即使是leader节点。<br>
官网：https://www.elastic.co/cn/elasticsearch/

* **ElasticSearch**<br>
Elasticsearch 是一个分布式、RESTful 风格的搜索和数据分析引擎，能够解决不断涌现出的各种用例。 作为 Elastic Stack 的核心，它集中存储您的数据，帮助您发现意料之中以及意料之外的情况。<br>
官网：https://etcd.io/

* **gRPC**<br>
gRPC是一个现代的开源高性能远程过程调用(Remote Procedure Call, RPC)框架，可以在任何环境中运行。通过对负载平衡、跟踪、运行状况检查和身份验证的可插拔支持，它可以有效地连接数据中心内和跨数据中心的服务。它也适用于分布式计算的最后一英里，将设备、移动应用程序和浏览器连接到后端服务。<br>
官网：https://grpc.io/

#### 云原生

* **微服务**<br>
微服务是一种软件架构风格，它是以专注于单一责任与功能的小型功能区块为基础，利用模块化的方式组合出复杂的大型应用程序，各功能区块使用与语言无关的API集相互通信。

* **DevOps**<br>
DevOps是一种重视“软件开发人员”和“IT运维技术人员”之间沟通合作的文化、运动或惯例。透过自动化“软件交付”和“架构变更”的流程，来使得构建、测试、发布软件能够更加地快捷、频繁和可靠。 

* **持续部署**<br>
持续部署，是一种软件工程方法，意指在软件开发流程中，以自动化方式，频繁而且持续性的，将软件部署到生产环境中，使软件产品能够快速的发展。
持续部署可以整合到持续整合与持续交付（Continuous delivery）的流程之中。

* **容器化**<br>
容器化是软件开发的一种方法，通过该方法可将应用程序或服务、其依赖项及其配置（抽象化为部署清单文件）一起打包为容器映像。 容器化应用程序可以作为一个单元进行测试，并可以作为容器映像实例部署到主机操作系统 (OS)。

### Step 3：如何高效地学习Go？

想要高效地的学习Golang，单单知道学习哪几个板块，是远远不够的。我们还需要将每个板块的知识点进一步细化。<br>
——**成功与失败之间，最重要的是不容忽视的细节**<br>

那么开始进一步完善之前的知识点：

<div  align=center>
  
<img width="70%" height="70%" src="https://user-images.githubusercontent.com/87457873/128875154-16e7ce3a-5817-4cb5-804a-1ad4b5bc5b5b.png"/>

</div>

#### 语法
* 语法基础
	* 错误处理
	* 包定义以及导入
	* 结构体
	* 反射原理
	* 值传递、引用传递、defer函数
* 并发编程
	* goroutine
	* 锁
	* 通道channel
	* runtime包
	* Context使用原则
* 网络编程
	* tcp/udp编程
	* http实现
	* websocket
* 源码掌握
	* GC机制
	* 调度器
	* 定时器
	* map与切片
* 第三方测试框架
	* goconvey
	* gostub
	* gomock
	* monkey

<div  align=center>
  
<img width="70%" height="70%" src="https://user-images.githubusercontent.com/87457873/128885748-5d075c80-c5d2-4f3b-bb2c-1939970b1305.png"/>

</div>

#### 中间件
* MySQL
  * golang的CRUD
  * jmorion/sqlx包
  * 连接池
  * 异步mysql
* Gin
  * RESTful API
  * URL查询参数
  * query接收数组和Map
  * 表单参数
  * 上传文件
  * 分组路由routel以及中间件授权
  * json、struct、xml、yaml、protobuf渲染
* Redis
  * go-redis
  * get/set/zset操作
  * 连接池
  * 分布式锁
* MongoDB
  * MongoDB-driver
  * BSON解析
  * CRUD操作
  * 文档管理
  * 连接池
* Kafka
  * saram包
  * 同步、异步
  * zstd压缩算法
  * 横向扩展
  * go实现生产消费者
  * topic和partition
  * 消息分发策略
  * 分区副本机制
* etcd
  * 原理
  * 分布式锁
  * etcd操作
  * 服务发现于注册
* ElasticSearch
  * es服务器
  * go- elasticsearch包
  * node于cluster
  * Index于Document
  * 检测与配置
* gRPC
  * protoc-gen-go开发包
  * .proto文件
  * gRPC Service Stub
  * rpc接口设计
  * 通信模式
  * 拦截器
  * 多路复用
  * 负载均衡
  * 安全认证

<div  align=center>
  
<img width="70%" height="70%" src="https://user-images.githubusercontent.com/87457873/128963340-173a3516-c0ec-470b-8546-ef6a34683066.png"/>

</div>

#### 后端开发
* 游戏后端
  * leaf框架
  * 网关、协议、日志、网络模块
* 流媒体Web后端
  * Restful接口设计
  * scheduler设计
  * apidefs结构体
  * mysql建库建表
* 小程序后端
  * 公众号开发流程
  * 微信消息接收与解析
  * 公众号验证URL+Token
  * 内网环境接口测试
  * 后端程序测试脚本
* goadmin后台权限管理系统
  * RESTful API设计
  * Gin框架
  * JWT认证
  * 支持Swagger文档
  * GORM对象关系映射
  * 基于Casbin的RBAC访问控制模型
* goim千万级高并发推送服务
  * 单个、多个、广播消息推送
  * 应用心跳、tcp、keepalive、http log pulling
  * 异步消息推送
  * 接入层多协议
  * 可拓扑架构
  * 注册发现服务
  * 消息协议（protobuf）
  * goim推送
  * grpc编程
* 腾讯云大数据
  * TBDS
  * 云数据仓库PostgreSQL
  * 弹性MapReduce
  * WeData数据开发平台

<div  align=center>
  
<img width="70%" height="70%" src="https://user-images.githubusercontent.com/87457873/128963600-fcca8120-e8e1-4a69-9562-e112e541e973.png"/>

</div>

#### 云原生

* 微服务
  * go-micro原理
  * rpc
  * 服务间同步
  * json/protobuf
* DevOps
  * 项目管理 CODING-PM
  * 测试管理 CODING-TM
  * 制品库 CODING-AR
  * 代码托管 CODING-CR
* 持续部署
  * spinnake
  * webhook外部对接
  * 蓝绿分布/金丝雀发布
  * SCF云函数
  * 快速回滚
* 容器化
  * Docker化部署
  * k8s集群
  * CVM云服务器
  * TKE容器服务


## 📚 资料包

<div  align=center>
  
![128686978-0d16ca8a-d070-4c3b-a4d9-640b265acc8d](https://user-images.githubusercontent.com/87457873/128688161-b51fbcb9-7e6e-4247-9911-4b94a35246b3.png)
  
</div>

---

### 📕 书籍

#### 入门

[《Go 入门指南》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%20%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97%E3%80%8B.pdf)

[《Go语言101》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E8%A8%80101%E3%80%8B.pdf)

《Go语言趣学指南》

《Go语言从入门到进阶实战》

[《Go语言学习笔记》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E8%A8%80%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%E3%80%8B.pdf)

[《Go语言入门经典》:hdcy](https://pan.baidu.com/s/15eMLovSIrdCLoILMjr4vBQ)

[《Go语言编程》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E8%A8%80%E7%BC%96%E7%A8%8B%E3%80%8B%E9%AB%98%E6%B8%85%E5%AE%8C%E6%95%B4%E7%89%88%E7%94%B5%E5%AD%90%E4%B9%A6.pdf)

[《Go语言实战》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E8%A8%80%E5%AE%9E%E6%88%98%E3%80%8B.epub)

[《Go Web 编程》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%20Web%20%E7%BC%96%E7%A8%8B%E3%80%8B.epub)

[《Go语言编程入门与实战技巧》:sgro](https://pan.baidu.com/s/1DlkBN8dRWmCHymyD4pJMCg)

#### 进阶

[《Go 语言圣经》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%20%E8%AF%AD%E8%A8%80%E5%9C%A3%E7%BB%8F%E3%80%8B.pdf)

[《Go专家编程》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGO%E4%B8%93%E5%AE%B6%E7%BC%96%E7%A8%8B%E3%80%8B.pdf)

[《Go 语法树入门》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E6%B3%95%E6%A0%91%E5%85%A5%E9%97%A8%E3%80%8B.pdf)

[《Go语言程序设计》:flnj](https://pan.baidu.com/s/11WpEqd9Fa7Ur5dH1XJdGtg)

[《Go语言高级编程》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E8%A8%80%E9%AB%98%E7%BA%A7%E7%BC%96%E7%A8%8B%E3%80%8B.pdf)

《Go语言核心编程》

《Go语言高并发与微服务实战》

[《Go并发编程实战》第2版：lsn0](https://pan.baidu.com/s/1fAjT7l16hY8ryXlknnSL-A)

[《Go语言并发之道》:6ppw](https://pan.baidu.com/s/1w6SbSkcjqFN1WiG2oS7XaA)

---

### 📖 文章

[Go语言之goroutine协程详解](https://github.com/0voice/Introduction-to-Golang/blob/main/%E6%96%87%E7%AB%A0/Go%E8%AF%AD%E8%A8%80%E4%B9%8Bgoroutine%E5%8D%8F%E7%A8%8B%E8%AF%A6%E8%A7%A3.md)

[Golang之sync.Pool对象池对象重用机制总结](https://github.com/0voice/Introduction-to-Golang/blob/main/%E6%96%87%E7%AB%A0/Golang%E4%B9%8Bsync.Pool%E5%AF%B9%E8%B1%A1%E6%B1%A0%E5%AF%B9%E8%B1%A1%E9%87%8D%E7%94%A8%E6%9C%BA%E5%88%B6%E6%80%BB%E7%BB%93.md)

[Golang的GC和内存逃逸](https://github.com/0voice/Introduction-to-Golang/blob/main/%E6%96%87%E7%AB%A0/Golang%E7%9A%84GC%E5%92%8C%E5%86%85%E5%AD%98%E9%80%83%E9%80%B8.md)

[GO语言之垃圾回收机制](https://github.com/0voice/Introduction-to-Golang/blob/main/%E6%96%87%E7%AB%A0/GO%E8%AF%AD%E8%A8%80%E4%B9%8B%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6.md)

[Go内存分配那些事，就这么简单](https://github.com/0voice/Introduction-to-Golang/blob/main/%E6%96%87%E7%AB%A0/Go%E5%86%85%E5%AD%98%E5%88%86%E9%85%8D%E9%82%A3%E4%BA%9B%E4%BA%8B%EF%BC%8C%E5%B0%B1%E8%BF%99%E4%B9%88%E7%AE%80%E5%8D%95%EF%BC%81.md)

[Go语言TCP Socket编程](https://github.com/0voice/Introduction-to-Golang/blob/main/%E6%96%87%E7%AB%A0/Go%E5%86%85%E5%AD%98%E5%88%86%E9%85%8D%E9%82%A3%E4%BA%9B%E4%BA%8B%EF%BC%8C%E5%B0%B1%E8%BF%99%E4%B9%88%E7%AE%80%E5%8D%95%EF%BC%81.md)

[从源码角度看 Golang 的调度](https://github.com/0voice/Introduction-to-Golang/blob/main/%E6%96%87%E7%AB%A0/%E4%BB%8E%E6%BA%90%E7%A0%81%E8%A7%92%E5%BA%A6%E7%9C%8B%20Golang%20%E7%9A%84%E8%B0%83%E5%BA%A6.md)

---

### 💽 视频

<br>

<div align=center>
  
title | Link
:------ | :------:
Go (Golang) Tutorial - Go语言入门教程 #1 ~ #22 | [百度网盘：btjy](https://pan.baidu.com/s/1gOHgMWDkJnDkMYtsg7fwGg)
GopherCon 2017- Russ Cox - The Future of Go | [百度网盘：zx1w](https://pan.baidu.com/s/1pFP8QbNxV6sBYZAPNelnRw)
GopherCon 2018- George Tankersley - Micro optimizing Go Code | [百度网盘：tzau](https://pan.baidu.com/s/1r7C6wsmQVnQkiWpWnDnLxA)
GopherCon 2018- Kat Zien - How Do You Structure Your Go Apps | [百度网盘：qmrq](https://pan.baidu.com/s/1BuM-Fd1_3NOawMpleB_cSA)
GopherCon 2018 Lightning Talk- Aidan Coyle - Lazy JSON Parsing | [百度网盘：g4f5](https://pan.baidu.com/s/1axakvOf4Yvk4VZX0nvOIDg)
GopherCon 2018 Lightning Talk- Alan Braithwaite - Web Session Management in Go | [百度网盘：2ibm](https://pan.baidu.com/s/16vqxJ4CDp3kXQEWgoQQZOQ)
GopherCon 2018 Lightning Talk- Hugo Torres - Talking to the Docker Socket | [百度网盘：unlh](https://pan.baidu.com/s/10y_32K0tV2l_LAe_ifE38w)
GopherCon 2019- Dave Cheney - Two Go Programs, Three Different Profiling Techniques | [百度网盘：kgjj](https://pan.baidu.com/s/1sbgzy7YFs_GbLcro299r2Q)
Let's Go - Why I'm moving to Go Programming Language - Why GoLang has a Promising Future | [百度网盘：ewon](https://pan.baidu.com/s/1HXRrbIQkkftU3x0KZymUpw)
Golang's Garbage - USENIX | [百度网盘：kkl4](https://pan.baidu.com/s/18HzVf2jLrgoTmbwCjRR0LA)
Golang REST API With Mux | [百度网盘：el4d](https://pan.baidu.com/s/1TrsNS6sgxFb8nj09HcP3rw)
Golang Crash Course | [百度网盘：v4jw](https://pan.baidu.com/s/1QXJW21hm-hOJ1hxMzkQoog)
Brad Fitzpatrick Go 1 11 and beyond | [百度网盘：yg9r](https://pan.baidu.com/s/1RsZHVCH3xI6hLfyCxCPS2g)
dotGo 2015 - Rob Pike - Simplicity is Complicated | [百度网盘：594c](https://pan.baidu.com/s/1RcfgpuCzYToBnaHMwfhjIw)
  
</div>

---

### ☁ 云原生

**这里我们讲云原生，主要目的是为了大家如何利用云原生技术，快速地使用go语言开发。而不是研究云原生本身的技术。**

我们以**腾讯云**为例，列举腾讯云为我们提供的云原生接口项目：（列举部分我们常见的）

* 实时音视频
  * 混流转码
  * 房间管理
  * 其他接口
  * 通话质量监控
* 云点播
  * [视频处理](https://github.com/0voice/Introduction-to-Golang/blob/main/%E4%BA%91%E5%8E%9F%E7%94%9F/%E8%85%BE%E8%AE%AF%E4%BA%91/%E4%BA%91%E7%82%B9%E6%92%AD/%E8%A7%86%E9%A2%91%E5%A4%84%E7%90%86%E7%9B%B8%E5%85%B3%E6%8E%A5%E5%8F%A3.md)
  * 参数模板
  * 其他接口
  * 任务流
  * 分发
  * 媒资管理
  * 视频上传
  * 事件通知
  * AI 样本管理
  * 视频分类
  * 任务管理
  * 域名管理
  * 数据统计
* 视频处理
  * 视频处理
  * 工作流管理
  * 其他接口
  * 解析事件通知
  * AI 样本管理
  * 参数模板
  * 任务管理
* 腾讯云剪
  * 平台管理
  * 项目管理
  * SaaS 平台管理
  * Headless媒体合成
  * 任务管理
  * 视频编辑项目
  * 媒体资源管理
  * 媒资授权
  * 团队管理
  * 分类管理
  * 登录态管理
  * 其他接口
* 智能编辑
  * 画质重生任务
  * 媒体质检任务
  * 编辑理解
  * 编辑处理
* 即时通信 IM
  * 获取云通信 IM 的 SDKAppid
* 短信
  * 短信统计
  * 发送短信
  * 拉取状态
  * 短信模板
  * 短信签名
* 分布式消息队列
  * 集群
  * 命名空间
  * 主题
  * 消息
  * CMQ管理
  * CMQ消息
  * 其他接口
  * 生产消费
* 消息队列 Ckafka
  * 主题
  * ACL
  * 实例
  * 路由
  * 其他接口
* 验证码
  * 控制台
  * 服务器
* 文本内容安全
* 图片内容检测
  * 图片内容审核
  * 图片内容检测
* 音频内容检测
  * 创建音频审核任务
  * 查看任务详情
  * 查看审核任务列表
  * 短音频识别接口
  * 取消任务
* 视频内容安全
  * 查看审核任务列表
  * 查看任务详情
  * 创建视频审核任务
  * 取消任务
* 文字识别
  * 通用文字识别
  * 卡证文字识别
  * 票据单据识别
  * 汽车场景识别
  * 行业文档识别
  * 智能扫码
  * 营业执照核验
  * 增值税发票核验
* 图像分析
  * 图像理解
  * 图像处理
  * 图像审核
  * 图像质量检测
* 智能识图
  * 商品识别
* 语音识别
  * 语音流异步识别
  * 录音文件识别
  * 一句话识别
  * 自学习
  * 热词
* 语音合成
  * 长文本语音合成
  * 通用语音合成
* 智能语音服务
  * 语音合成
  * 其他接口
  * 语音识别
* 云直播
  * 水印管理
  * 证书管理
  * 录制管理
  * 直播流管理
  * 延播管理
  * 直播拉流
  * 直播转码
  * 截图鉴黄
  * 鉴权管理
  * 域名管理
  * 直播回调
  * 统计查询
  * 实时日志
  * 直播混流
* 小程序 · 云直播
  * live
  * 其他接口
  * 直播
* 人脸识别
  * 人脸检测与分析
  * 五官定位
  * 人脸比对
  * 人员库管理
  * 人脸搜索
  * 人脸验证
  * 人脸静态活体检测（高精度版）
  * 人脸静态活体检测
  * 人员查重
* 内容安全
  * 内容安全
  * 图片内容安全
  * 文本内容安全
  * 自定义识别
* 自然语言处理
  * 向量技术
  * 词法分析
  * 句法分析
  * 篇章分析
  * 知识图谱
  * 对话机器人<br>
......

---

### 📄 论文与理论分析

---

### 🏗 开源项目

#### 王者段位

项目 | 简介| 地址
-------|-------------------|---------------
docker | 无人不知的虚拟华平台，开源的应用容器引擎,借助该引擎，开发者可以打包他们的应用，移植到任何平台上。| https://github.com/docker/docker
golang | go本身，也是用go语言实现的，包括他的编译器，要研究go源代码的可以看此项目录 | https://github.com/golang/go
kubernetes | Google出品，用于调度和管理docker的开源容器管理系统，利用他，可以方便的管理你的docker实例，哪怕非常多，也是目前最流行的docker管理系统。 | https://github.com/kubernetes/kubernetes
gogs |一款基于git的代码托管系统，类似于github和gitlab，不过其小巧易用，功能强大，部署方便，也有不少用户在使用。 | https://github.com/gogits/gogs
syncthing | 开源的文件同步系统,它使用了其独有的对等自由块交换协议,速度很快,据说可以替换BitTorrent Sync。 |https://github.com/syncthing/syncthing
grafana |一款开源监控度量的看板系统，可以接Graphite,Elasticsearch,InfluxDB等数据源，定制化很高。 | https://github.com/grafana/grafana
etcd | 一款分布式的，可靠的K-V存储系统，使用简单，速度快，又安全。 |https://github.com/coreos/etcd
hub |一款更便捷使用github的工具，包装并且扩展了git，提供了很多特性和功能，使用和git差不多。 |https://github.com/github/hub
influxdb | 可伸缩的数据库，使用场景主要用来存储测量数据，事件点击以及其他等实时分析数据，用来做监控性能很不错。 |https://github.com/influxdata/influxdb
caddy |快速的，跨平台的HTTP/2 Web服务器。 |https://github.com/mholt/caddy
beego | 国产开源的高性能Web框架，让你快速的开发Go Web应用服务，谢大主笔。 |https://github.com/astaxie/beego
martini |也是一款不错的Web框架。 |https://github.com/go-martini/martini
cayley |Google开源的图数据库，这是一个NoSql数据库，适合处理复杂的，但是结构化低的数据,适用于社交网络，推荐系统等。|https://github.com/cayleygraph/cayley
nsq |一款开源的实时的，分布式的消息中间件系统。|https://github.com/nsqio/nsq
codis | Codis是一个分布式Redis解决方案,其实就是一个数据库代理，让你在使用Redis集群的时候，就像使用单机版的Redis是一样的，对开发者透明。 |https://github.com/CodisLabs/codis
delve |这个Go开发者都知道，一款go应用开发的调试工具。 |https://github.com/derekparker/delve
cobra |cobra是一个命令行go库，可以让你创建非常强大的，现代的CLI命令行应用。|https://github.com/spf13/cobra


---

### 🖼 大厂实战分享ppt

* [Go in TiDB](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Go%20in%20TiDB.pdf)
* [Go 如何助力企业进行微服务转型](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Go%20%E5%A6%82%E4%BD%95%E5%8A%A9%E5%8A%9B%E4%BC%81%E4%B8%9A%E8%BF%9B%E8%A1%8C%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%BD%AC%E5%9E%8B.pdf)
* [Golang 内存管理探微&mdash&mdash，如何高效使用 Golang 内存以及腾讯云实战-杨晖](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Golang%20%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86%E6%8E%A2%E5%BE%AE%26mdash%26mdash%EF%BC%8C%E5%A6%82%E4%BD%95%E9%AB%98%E6%95%88%E4%BD%BF%E7%94%A8%20Golang%20%E5%86%85%E5%AD%98%E4%BB%A5%E5%8F%8A%E8%85%BE%E8%AE%AF%E4%BA%91%E5%AE%9E%E6%88%98-%E6%9D%A8%E6%99%96.pdf)
* [Golang主动式内存缓存的优化探索之路](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Golang%E4%B8%BB%E5%8A%A8%E5%BC%8F%E5%86%85%E5%AD%98%E7%BC%93%E5%AD%98%E7%9A%84%E4%BC%98%E5%8C%96%E6%8E%A2%E7%B4%A2%E4%B9%8B%E8%B7%AF.pdf)
* [Golang在百万亿搜索引擎中的应用](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Golang%E5%9C%A8%E7%99%BE%E4%B8%87%E4%BA%BF%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E%E4%B8%AD%E7%9A%84%E5%BA%94%E7%94%A8.pdf)
* [Golang在阿里巴巴调度系统Sigma中的实践](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Golang%E5%9C%A8%E9%98%BF%E9%87%8C%E5%B7%B4%E5%B7%B4%E8%B0%83%E5%BA%A6%E7%B3%BB%E7%BB%9FSigma%E4%B8%AD%E7%9A%84%E5%AE%9E%E8%B7%B5.pdf)
* [Go在大数据开发中的经验总结](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Go%E5%9C%A8%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%BC%80%E5%8F%91%E4%B8%AD%E7%9A%84%E7%BB%8F%E9%AA%8C%E6%80%BB%E7%BB%93.pdf)
* [Go在探探后端的工程实践](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Go%E5%9C%A8%E6%8E%A2%E6%8E%A2%E5%90%8E%E7%AB%AF%E7%9A%84%E5%B7%A5%E7%A8%8B%E5%AE%9E%E8%B7%B5.pdf)
* [Go在证券行情系统中的应用](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Go%E5%9C%A8%E8%AF%81%E5%88%B8%E8%A1%8C%E6%83%85%E7%B3%BB%E7%BB%9F%E4%B8%AD%E7%9A%84%E5%BA%94%E7%94%A8.pdf)
* [Go微服务实战](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Go%E5%BE%AE%E6%9C%8D%E5%8A%A1%E5%AE%9E%E6%88%98.pdf)
* [Go打造亿级实时分布式平台](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Go%E6%89%93%E9%80%A0%E4%BA%BF%E7%BA%A7%E5%AE%9E%E6%97%B6%E5%88%86%E5%B8%83%E5%BC%8F%E5%B9%B3%E5%8F%B0_2.pdf)
* [Go语言在讯联扫码支付系统中的成功实践](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Go%E8%AF%AD%E8%A8%80%E5%9C%A8%E8%AE%AF%E8%81%94%E6%89%AB%E7%A0%81%E6%94%AF%E4%BB%98%E7%B3%BB%E7%BB%9F%E4%B8%AD%E7%9A%84%E6%88%90%E5%8A%9F%E5%AE%9E%E8%B7%B5.pdf)
* [Go语言在证券期货行情系统中的实践](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Go%E8%AF%AD%E8%A8%80%E5%9C%A8%E8%AF%81%E5%88%B8%E6%9C%9F%E8%B4%A7%E8%A1%8C%E6%83%85%E7%B3%BB%E7%BB%9F%E4%B8%AD%E7%9A%84%E5%AE%9E%E8%B7%B5.pdf)
* [Go语言的抢占式调度](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Go%E8%AF%AD%E8%A8%80%E7%9A%84%E6%8A%A2%E5%8D%A0%E5%BC%8F%E8%B0%83%E5%BA%A6.pdf)
* [Improving Go Backend Developer Experience in Grab](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Improving%20Go%20Backend%20Developer%20Experience%20in%20Grab.pdf)
* [Processing XML and Spreadsheet Data in Go](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Processing%20XML%20and%20Spreadsheet%20Data%20in%20Go.pdf)
* [The Zen of Go](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/The%20Zen%20of%20Go.pdf)
* [Tracing in TiDB](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/Tracing%20in%20TiDB.pdf)
* [goplus-gopher-china](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/goplus-gopher-china.pdf)
* [云原生技术在2B交付中的实践](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E4%BA%91%E5%8E%9F%E7%94%9F%E6%8A%80%E6%9C%AF%E5%9C%A82B%E4%BA%A4%E4%BB%98%E4%B8%AD%E7%9A%84%E5%AE%9E%E8%B7%B5.pdf)
* [利用夜莺扩展能力打造全方位监控系统](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E5%88%A9%E7%94%A8%E5%A4%9C%E8%8E%BA%E6%89%A9%E5%B1%95%E8%83%BD%E5%8A%9B%E6%89%93%E9%80%A0%E5%85%A8%E6%96%B9%E4%BD%8D%E7%9B%91%E6%8E%A7%E7%B3%BB%E7%BB%9F.pdf)
* [基于 Golang 构建高可扩展的云原生 PaaS 平台](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E5%9F%BA%E4%BA%8E%20Golang%20%E6%9E%84%E5%BB%BA%E9%AB%98%E5%8F%AF%E6%89%A9%E5%B1%95%E7%9A%84%E4%BA%91%E5%8E%9F%E7%94%9F%20PaaS%20%E5%B9%B3%E5%8F%B0.pdf)
* [基于Kubernetes的私有云实战](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E5%9F%BA%E4%BA%8EKubernetes%E7%9A%84%E7%A7%81%E6%9C%89%E4%BA%91%E5%AE%9E%E6%88%98.pdf)
* [大规模场景下Kubernetes Service负载均衡性能优化](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E5%A4%A7%E8%A7%84%E6%A8%A1%E5%9C%BA%E6%99%AF%E4%B8%8BKubernetes%20Service%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96.pdf)
* [天猫DevOps转型实践](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E5%A4%A9%E7%8C%ABDevOps%E8%BD%AC%E5%9E%8B%E5%AE%9E%E8%B7%B5.pdf)
* [如何用Go模拟CPU](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E5%A6%82%E4%BD%95%E7%94%A8Go%E6%A8%A1%E6%8B%9FCPU.pdf)
* [字节跳动在 Go 网络库上的实践](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E5%AD%97%E8%8A%82%E8%B7%B3%E5%8A%A8%E5%9C%A8%20Go%20%E7%BD%91%E7%BB%9C%E5%BA%93%E4%B8%8A%E7%9A%84%E5%AE%9E%E8%B7%B5.pdf)
* [深入Go Module](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E6%B7%B1%E5%85%A5Go%20Module.pdf)
* [深入理解BFE](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3BFE.pdf)
* [罗辑思维Go语言微服务改造实践](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E7%BD%97%E8%BE%91%E6%80%9D%E7%BB%B4Go%E8%AF%AD%E8%A8%80%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%94%B9%E9%80%A0%E5%AE%9E%E8%B7%B5.pdf)
* [谈如何构建易于拆分的单体应用](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E8%B0%88%E5%A6%82%E4%BD%95%E6%9E%84%E5%BB%BA%E6%98%93%E4%BA%8E%E6%8B%86%E5%88%86%E7%9A%84%E5%8D%95%E4%BD%93%E5%BA%94%E7%94%A8.pdf)
* [跨境电商的Go服务治理实践](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E8%B7%A8%E5%A2%83%E7%94%B5%E5%95%86%E7%9A%84Go%E6%9C%8D%E5%8A%A1%E6%B2%BB%E7%90%86%E5%AE%9E%E8%B7%B5.pdf)
* [阿里巴巴新一代基于 Go 的云原生应用引擎实践](https://github.com/0voice/Introduction-to-Golang/blob/main/Golang%20PPT/%E9%98%BF%E9%87%8C%E5%B7%B4%E5%B7%B4%E6%96%B0%E4%B8%80%E4%BB%A3%E5%9F%BA%E4%BA%8E%20Go%20%E7%9A%84%E4%BA%91%E5%8E%9F%E7%94%9F%E5%BA%94%E7%94%A8%E5%BC%95%E6%93%8E%E5%AE%9E%E8%B7%B5.pdf)
