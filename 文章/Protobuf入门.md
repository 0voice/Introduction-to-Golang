在Kafka中，发送的消息是字节数组，因此就需要一个方法来将消息对象序列化为字节数组，在消费者端再反序列化为对象。最常用的序列化格式就是JSON了。虽然JSON对人类非常友好，但是对于机器来说，更容易进行序列化和反序列化的格式还是二进制的格式。

Protobuf（Protocol buffers）是由Google开发的一种二进制协议，用于对结构化数据进行序列化和反序列化。这种格式占用空间更少，更加简单易于维护，同时拥有更好的性能。对于机器之间的通信，Protobuf是比XML和JSON等格式更好的一种选择。

Protobuf的使用相比之下更加复杂，需要编写.proto格式的文件来定义数据格式，之后通过protoc编译器将其编译到对应的语言，之后再在程序中引用。

# 使用
## 安装

首先就是安装protoc编译器，这个编译器可以直接到github上下载二进制包，解压到对应位置并设置PATH即可。

之后就是安装对应语言的客户端，对于golang，执行下面两条语句安装就可以了：

    go get github.com/golang/protobuf/proto
    go get github.com/golang/protobuf/protoc-gen-go
    
## 编译
之后新建一个.proto文件，在其中定义消息的格式：

    syntax = "proto3";
    package test;

    option go_package = "proto/test";

    message Award {
        int64 uid = 1;
        int64 awardId = 2;
        string userName = 3;
    }
    
然后使用protoc编译器将其编译到go文件就行了：

    protoc --go_out=. proto/*.proto
    
在go程序中引入生成的包就可以进行序列化和反序列化了：

```go
import pb "proto/test"

func marshal() {
    award := &pb.Award{
		Uid:      628,
		AwardId:  1,
		UserName: "Haruka",
	}
	msg, err := proto.Marshal(award)
}

func unmarshal() {
    award := &pb.Award{}
	if err := proto.Unmarshal(msg.Value, award); err != nil {
		panic(err)
	}
}
```

# proto3语言
在.proto文件的第一行就是syntax = "proto3";，用于声明该文件是proto3版本的。之后可以声明package用于避免命名冲突，最后就可以定义message了。

## 字段ID

message的每个字段都要分配一个唯一的ID，最小是1最大是2^29 - 1，同时不能使用 1900019999的ID。ID可以任意分配，但是115只会占用1个字节，而16~2047会占用2个字节，因此应尽量从小开始分配，并将小的分配给最经常出现的字段。

可以使用reserved来对字段名和ID进行保留，一般用于为未来新增字段保留，或者保留删除字段来避免之后的字段使用，防止发生冲突：

    message Foo {
      reserved 2, 15, 9 to 11;
      reserved "foo", "bar";
    }

## 字段类型

字段的类型默认是singular，也就是只能出现一次或0次，如果在字段声明前加上repeated的话就可以出现多次。

protobuf的标量类型有以下几种：double float int32 int64 uint32 uint64 sint32 sint64 fixed32 fixed64 sfixed32 sfixed64 bool string bytes。

protobuf中还可以使用枚举和复合类型。例如可以通过import "google/protobuf/timestamp.proto"来使用timestamp类型。

protobuf数据类型的默认值规则如下，如果一个字段被设置为默认值，则其不会被序列化：

* string bytes类型默认为空
* bool类型默认为false
* 数值类型默认为0
* 枚举类型默认为第一个，即0
* 复合类型取决于语言

### 枚举类型

```
message SearchRequest {
  enum Corpus {
    option allow_alias = true;
    UNIVERSAL = 0;
    WEB = 1;
    TEST = 1;
    IMAGES = 2;
  }
  Corpus corpus = 4;
}
```

通过enum来声明一个枚举，注意枚举中必须要有0值用来作为默认值，同时0值应该是第一个元素，以与proto2兼容。通过option allow_alias = true;来允许两个枚举元素有相同的值，即这两个元素可以相互替代。

### 复合类型
在一个message中，可以嵌入其他的message作为复合类型：

    message SearchResponse {
      repeated Result results = 1;
    }

    message Result {
      string url = 1;
      string title = 2;
      repeated string snippets = 3;
    }
    
在protobuf中还有其他高级类型，如Any oneof map等，就不详细介绍了。
