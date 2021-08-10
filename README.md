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

```
|– AUTHORS — 文件，官方 Go语言作者列表
|– CONTRIBUTORS — 文件，第三方贡献者列表
|– LICENSE — 文件，Go语言发布授权协议
|– PATENTS — 文件，专利
|– README — 文件，README文件，大家懂的。提一下，经常有人说：Go官网打不开啊，怎么办？其实，在README中说到了这个。
该文件还提到，如果通过二进制安装，需要设置GOROOT环境变量；如果你将Go放在了/usr/local/go中，则可以不设置该环境变
量（Windows下是C:\go）。当然，建议不管什么时候都设置GOROOT。另外，确保$GOROOT/bin在PATH目录中。
|– VERSION — 文件，当前Go版本
|– api — 目录，包含所有API列表，方便IDE使用
|– doc — 目录，Go语言的各种文档，官网上有的，这里基本会有，这也就是为什么说可以本地搭建”官网”。
这里面有不少其他资源，比如gopher图标之类的。
|– favicon.ico — 文件，官网logo
|– include — 目录，Go 基本工具依赖的库的头文件
|– lib — 目录，文档模板
|– misc — 目录，其他的一些工具，相当于大杂烩，大部分是各种编辑器的Go语言支持，还有cgo的例子等
|– robots.txt — 文件，搜索引擎robots文件
|– src — 目录，Go语言源码：基本工具（编译器等）、标准库
|– test — 目录，包含很多测试程序（并非_test.go方式的单元测试，而是包含main包的测试），包括一些fixbug测试。
可以通过这个学到一些特性的使用。
```
### Step 2：Go需要学习哪些内容？

### Step 3：如何高效地学习Go？

## 📚 资料包

<div  align=center>
  
![128686978-0d16ca8a-d070-4c3b-a4d9-640b265acc8d](https://user-images.githubusercontent.com/87457873/128688161-b51fbcb9-7e6e-4247-9911-4b94a35246b3.png)
  
</div>

### 📕 书籍

#### 入门

[《Go 入门指南》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%20%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97%E3%80%8B.pdf)

[《Go语言101》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E8%A8%80101%E3%80%8B.pdf)

《Go语言趣学指南》

《Go语言从入门到进阶实战》

[《Go语言学习笔记》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E8%A8%80%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%E3%80%8B.pdf)

《Go语言入门经典》

[《Go语言编程》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E8%A8%80%E7%BC%96%E7%A8%8B%E3%80%8B%E9%AB%98%E6%B8%85%E5%AE%8C%E6%95%B4%E7%89%88%E7%94%B5%E5%AD%90%E4%B9%A6.pdf)

[《Go语言实战》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E8%A8%80%E5%AE%9E%E6%88%98%E3%80%8B.epub)

[《Go Web 编程》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%20Web%20%E7%BC%96%E7%A8%8B%E3%80%8B.epub)

《Go语言编程入门与实战技巧》

#### 进阶

[《Go 语言圣经》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%20%E8%AF%AD%E8%A8%80%E5%9C%A3%E7%BB%8F%E3%80%8B.pdf)

[《Go专家编程》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGO%E4%B8%93%E5%AE%B6%E7%BC%96%E7%A8%8B%E3%80%8B.pdf)

[《Go 语法树入门》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E6%B3%95%E6%A0%91%E5%85%A5%E9%97%A8%E3%80%8B.pdf)

《Go语言编程之旅》

[《Go语言高级编程》](https://github.com/0voice/Introduction-to-Golang/blob/main/%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D/%E3%80%8AGo%E8%AF%AD%E8%A8%80%E9%AB%98%E7%BA%A7%E7%BC%96%E7%A8%8B%E3%80%8B.pdf)

《Go语言核心编程》

《Go语言高并发与微服务实战》

《Go并发编程实战》

《Go语言并发之道》

### 📖 文章

### 💽 视频

### ☁ 云原生

### 📄 论文与理论分析

### 🏗 开源框架

### 🖼 大厂实战分享ppt



























