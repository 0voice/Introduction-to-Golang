# Go常用命令

Go语言自带有一套完整的命令操作工具，可以通过在命令行中执行`go`来查看它们

## go build

这个命令主要用于编译代码。在包的编译过程中，若有必要，会同时编译与之相关联的包。

- 如果是普通包，就像编写的`mymath`包那样，当执行`go build`之后，它不会产生任何文件。如果需要在`$GOPATH/pkg`下生成相应的文件，那就得执行`go install`。
- 如果是`main`包，当执行`go build`之后，它就会在当前目录下生成一个可执行文件。如果需要在`$GOPATH/bin`下生成相应的文件，需要执行`go install`，或者使用`go build -o 路径/a.exe`。

- 如果某个项目文件夹下有多个文件，而只想编译某个文件，就可在`go build`之后加上文件名，例如`go build a.go`；`go build`命令默认会编译当前目录下的所有go文件。
- 也可以指定编译输出的文件名。例如1.2节中的`mathapp`应用，可以指定`go build -o astaxie.exe`，默认情况是package名(非main包)，或者是第一个源文件的文件名(main包)。
  （注：实际上，package名在Go语言规范中指代码中“package”后使用的名称，此名称可以与文件夹名不同。默认生成的可执行文件名是文件夹名。）

- go build会忽略目录下以“_”或“.”开头的go文件。
- 如果源代码针对不同的操作系统需要不同的处理，那么可以根据不同的操作系统后缀来命名文件。例如有一个读取数组的程序，它对于不同的操作系统可能有如下几个源文件：
  array_linux.go
  array_darwin.go
  array_windows.go
  array_freebsd.go
  `go build`的时候会选择性地编译以系统名结尾的文件（Linux、Darwin、Windows、Freebsd）。例如Linux系统下面编译只会选择array_linux.go文件，其它系统命名后缀文件全部忽略。

参数的介绍

- `-o` 指定输出的文件名，可以带上路径，例如 `go build -o a/b/c`
- `-i` 安装相应的包，编译+`go install`

- `-a` 更新全部已经是最新的包的，但是对标准包不适用
- `-n` 把需要执行的编译命令打印出来，但是不执行，这样就可以很容易的知道底层是如何运行的

- `-p n` 指定可以并行可运行的编译数目，默认是CPU数目
- `-race` 开启编译的时候自动检测数据竞争的情况，目前只支持64位的机器

- `-v` 打印出来正在编译的包名
- `-work` 打印出来编译时候的临时文件夹名称，并且如果已经存在的话就不要删除

- `-x` 打印出来执行的命令，其实就是和`-n`的结果类似，只是这个会执行
- `-ccflags 'arg list'` 传递参数给5c, 6c, 8c 调用

- `-compiler name` 指定相应的编译器，gccgo还是gc
- `-gccgoflags 'arg list'` 传递参数给gccgo编译连接调用

- `-gcflags 'arg list'` 传递参数给5g, 6g, 8g 调用
- `-installsuffix suffix` 为了和默认的安装包区别开来，采用这个前缀来重新安装那些依赖的包，`-race`的时候默认已经是`-installsuffix race`,大家可以通过`-n`命令来验证

- `-ldflags 'flag list'` 传递参数给5l, 6l, 8l 调用
- `-tags 'tag list'` 设置在编译的时候可以适配的那些tag，详细的tag限制参考里面的 Build Constraints

## go clean

这个命令是用来移除当前源码包和关联源码包里面编译生成的文件。这些文件包括

```bash
_obj/            旧的object目录，由Makefiles遗留
_test/           旧的test目录，由Makefiles遗留
_testmain.go     旧的gotest文件，由Makefiles遗留
test.out         旧的test记录，由Makefiles遗留
build.out        旧的test记录，由Makefiles遗留
*.[568ao]        object文件，由Makefiles遗留
DIR(.exe)        由go build产生
DIR.test(.exe)   由go test -c产生
MAINFILE(.exe)   由go build MAINFILE.go产生
*.so             由 SWIG 产生
```

一般都是利用这个命令清除编译文件，然后github递交源码，在本机测试的时候这些编译文件都是和系统相关的，但是对于源码管理来说没必要。

```bash
$ go clean -i -n
cd /Users/astaxie/develop/gopath/src/mathapp
rm -f mathapp mathapp.exe mathapp.test mathapp.test.exe app app.exe
rm -f /Users/astaxie/develop/gopath/bin/mathapp
```

参数介绍

- `-i` 清除关联的安装的包和可运行文件，也就是通过go install安装的文件
- `-n` 把需要执行的清除命令打印出来，但是不执行，这样就可以很容易的知道底层是如何运行的

- `-r` 循环的清除在import中引入的包
- `-x` 打印出来执行的详细命令，其实就是`-n`打印的执行版本

## go fmt

有过C/C++经验的读者会知道,一些人经常为代码采取K&R风格还是ANSI风格而争论不休。在go中，代码则有标准的风格。由于之前已经有的一些习惯或其它的原因常将代码写成ANSI风格或者其它更合适自己的格式，这将为人们在阅读别人的代码时添加不必要的负担，所以go强制了代码格式（比如左大括号必须放在行尾），不按照此格式的代码将不能编译通过，为了减少浪费在排版上的时间，go工具集中提供了一个`go fmt`命令 它可以格式化写好的代码文件，写代码的时候不需要关心格式，只需要在写完之后执行`go fmt <文件名>.go`，代码就被修改成了标准格式，但是平常很少用到这个命令，因为开发工具里面一般都带了保存时候自动格式化功能，这个功能其实在底层就是调用了`go fmt`。接下来将讲述两个工具，这两个工具都自带了保存文件时自动化`go fmt`功能。

使用go fmt命令，其实是调用了gofmt，而且需要参数-w，否则格式化结果不会写入文件。gofmt -w -l src，可以格式化整个项目。

所以go fmt是gofmt的上层一个包装的命令，想要更多的个性化的格式化可以参考 go fmt

gofmt的参数介绍

- `-l` 显示那些需要格式化的文件
- `-w` 把改写后的内容直接写入到文件中，而不是作为结果打印到标准输出。

- `-r` 添加形如“a[b:len(a)] -> a[b:]”的重写规则，方便做批量替换
- `-s` 简化文件中的代码

- `-d` 显示格式化前后的diff而不是写入文件，默认是false
- `-e` 打印所有的语法错误到标准输出。如果不使用此标记，则只会打印不同行的前10个错误。

- `-cpuprofile` 支持调试模式，写入相应的cpufile到指定的文件

## go get

这个命令是用来动态获取远程代码包的，目前支持的有BitBucket、GitHub、Google Code和Launchpad。这个命令在内部实际上分成了两步操作：第一步是下载源码包，第二步是执行`go install`。下载源码包的go工具会自动根据不同的域名调用不同的源码工具，对应关系如下：

```bash
BitBucket (Mercurial Git)
GitHub (Git)
Google Code Project Hosting (Git, Mercurial, Subversion)
Launchpad (Bazaar)
```

所以为了`go get` 能正常工作，必须确保安装了合适的源码管理工具，并同时把这些命令加入PATH中。其实`go get`支持自定义域名的功能，具体参见`go help remote`。

参数介绍：

- `-d` 只下载不安装
- `-f` 只有在包含了`-u`参数的时候才有效，不让`-u`去验证import中的每一个都已经获取了，这对于本地fork的包特别有用

- `-fix` 在获取源码之后先运行fix，然后再去做其他的事情
- `-t` 同时也下载需要为运行测试所需要的包

- `-u` 强制使用网络去更新包和它的依赖包
- `-v` 显示执行的命令

从 Go 1.16 起，不推荐通过 go get 来安装包（主要是说安装可执行文件），也就是说，go get 应该只是用来下载包，而且将来版本可能会给该命令始终加上 `-d` 标志。

看一个实际的例子：

在本地通过源码安装 Go 的调试器 Delve，可以这么做：

```bash
$ go get github.com/go-delve/delve/cmd/dlv
```

因为 go get 会下载、编译并安装包（如果有 main 包）。

Go 1.16 建议这么使用 go get：

```bash
$ go get -d github.com/go-delve/delve/cmd/dlv
```

这只会下载 delve，并不会构建和安装，而且将来 go get 只会用来下载。因此，还需要手动执行安装。

## go install

这个命令在内部实际上分成了两步操作：第一步是生成结果文件(可执行文件或者.a包)，第二步会把编译好的结果移到`$GOPATH/pkg`或者`$GOPATH/bin`。

`go install` 会将包编译成 `.a` 文件并安装到 `$GOPATH/pkg/$GOOS_$GOARCH` 下；如果是 main 包，会编译并生成可执行文件安装到 `$GOPATH/bin` 目录下（如果设置了 `$GOBIN`，则会安装到 `$GOBIN` 下 ）。这也是和 go build 不同之处。

参数支持`go build`的编译参数。一般只要记住一个参数`-v`就好了，这个随时随地的可以查看底层的执行信息。

Module 没启用时，和 GOPATH 年代的作用是一样的。当启用 Module 模式时，go install 对普通包（非 main 包）不再安装（即没有了 `pkg/$GOOS_$GOARCH`），这和 go build 一样了。而对于 main 包，会将生成的可执行文件安装到 `$GOBIN` 目录下（`$GOBIN` 的默认值是 `$GOPATH/bin`，如果 `$GOPATH` 没有设置，则是 `$HOME/go/bin`）。

从 Go 1.16 起，go install 可以接受带有版本后缀的参数（例如 go install example.com/cmd@v1.0.0）。这将导致 go install 以模块感知模式构建和安装包，而忽略当前目录或任何父目录（如果有）中的 go.mod 文件。这对于在不影响主模块依赖性的情况下安装可执行文件很有用。

- 如果要安装第三方库的可执行文件，比如上面的 Delve，使用 go install，但需要带上版本后缀，比如 @latest；
- 普通的库，继续使用 go get，建议加上 -d 标志；

注意：虽然 go install 一个普通的第三方包（不过必须带上版本后缀）也会下载对应的包，但不会修改 go.mod，这和 go get 是不同的。

## go test

执行这个命令，会自动读取源码目录下面名为`*_test.go`的文件，生成并运行测试用的可执行文件。输出的信息类似

```bash
ok   archive/tar   0.011s
FAIL archive/zip   0.022s
ok   compress/gzip 0.033s
...
```

默认的情况下，不需要任何的参数，它会自动把源码包下面所有test文件测试完毕，当然也可以带上参数，详情请参考`go help testflag`

这里介绍几个常用的参数：

- `-bench regexp` 执行相应的benchmarks，例如 `-bench=.`
- `-cover` 开启测试覆盖率

- `-run regexp` 只运行regexp匹配的函数，例如 `-run=Array` 那么就执行包含有Array开头的函数
- `-v` 显示测试的详细命令

## go tool

`go tool`下面下载聚集了很多命令，这里只介绍两个，fix和vet

- `go tool fix .` 用来修复以前老版本的代码到新版本，例如go1之前老版本的代码转化到go1,例如API的变化
- `go tool vet directory|files` 用来分析当前目录的代码是否都是正确的代码,例如是不是调用fmt.Printf里面的参数不正确，例如函数里面提前return了然后出现了无用代码之类的。

## go generate

这个命令是从Go1.4开始才设计的，用于在编译前自动化生成某类代码。`go generate`和`go build`是完全不一样的命令，通过分析源码中特殊的注释，然后执行相应的命令。这些命令都是很明确的，没有任何的依赖在里面。而且大家在用这个之前心里面一定要有一个理念，这个`go generate`是给开发者用的，不是给使用这个包的人用的，是方便生成一些代码的。

举一个简单的例子，例如经常会使用`yacc`来生成代码，那么常用这样的命令：

```bash
go tool yacc -o gopher.go -p parser gopher.y
```

-o 指定了输出的文件名， -p指定了package的名称，这是一个单独的命令，如果想让`go generate`来触发这个命令，那么就可以在当前目录的任意一个`xxx.go`文件里面的任意位置增加一行如下的注释：

```bash
//go:generate go tool yacc -o gopher.go -p parser gopher.y
```

这里注意了，`//go:generate`是没有任何空格的，这其实就是一个固定的格式，在扫描源码文件的时候就是根据这个来判断的。

所以可以通过如下的命令来生成，编译，测试。如果`gopher.y`文件有修改，那么就重新执行`go generate`重新生成文件就好。

```bash
$ go generate
$ go build
$ go test
```

## godoc

在Go1.2版本之前还支持`go doc`命令，但是之后全部移到了godoc这个命令下，需要这样安装`go get golang.org/x/tools/cmd/godoc`

很多人说go不需要任何的第三方文档，例如chm手册之类的，因为它内部就有一个很强大的文档工具。

如何查看相应package的文档呢？

 例如builtin包，那么执行`godoc builtin`

 如果是http包，那么执行`godoc net/http`

 查看某一个包里面的函数，那么执行`godoc fmt Printf`

 也可以查看相应的代码，执行`godoc -src fmt Printf`

通过命令在命令行执行 godoc -http=:端口号 比如`godoc -http=:8080`。然后在浏览器中打开`127.0.0.1:8080`，将会看到一个golang.org的本地copy版本，通过它可以查询pkg文档等其它内容。如果设置了GOPATH，在pkg分类下，不但会列出标准包的文档，还会列出本地`GOPATH`中所有项目的相关文档，这对于经常被墙的用户来说是一个不错的选择。

## 其它命令

go还提供了其它很多的工具，例如下面的这些工具

```bash
go version 查看go当前的版本
go env 查看当前go的环境变量
go list 列出当前全部安装的package
go run 编译并运行Go程序
```
