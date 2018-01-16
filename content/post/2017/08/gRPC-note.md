+++
title = "gRPC使用手记"
date = 2017-08-22T23:44:21+08:00
draft = false
slug = ""
categories = ["分布式","使用手记"]
tags = ["gRPC","RPC"]
toc = true
+++

## gRPC是什么?
gRPC是由谷歌开发的一款高性能，通用的开源RPC框架，基于HTTP/2协议标准设计.支持全双工、双向流、流控制、头部压缩单连接上的多服用请求等特性。目前提供C、Java和Go三个语言版本。  三个项目版本地址分别是:
[C-gRPC](https://github.com/grpc/grpc)  
[Go-gRPC](https://github.com/grpc/grpc-go)  
[JAVA-gRPC](https://github.com/grpc/grpc-java)  

## 为什么要选用它?
1.gRPC基于HTTP/2设计，相对于其他RPC框架，其功能更强大，真正实现全双工，支持头部压缩和多复用请求。因此能够很好的节省带宽和TCP链接次数，节省CPU使用，对于移动端的使用非常适用，也能提高云端的服务和Web应用性能。  
2.支持Protobuf，在社区上看到不少人是出于Protobuf而选择gPRC.Protobuf拥有灵活、高效、结构化的特点，相比于其他传统的序列化方式，它更小、更快、更简单。其数据结构化一次可以支持多语言使用。支持不同版本的数据结构数据传递，实现数据结构的前向兼容。  
3.拥有Golang版本（笔者选择的原因之一）
## 使用过程
### Protobuf 3.0
安装protobuf
```go
go get -u github.com/golang/protobuf/proto // golang protobuf 库
```
1、安装protoc工具，用于根据protobuf文件生成出Go代码
```
go get -u github.com/golang/protobuf/protoc-gen-go //protoc --go_out 工具
```
2、编写Protobuf3文件,可参考 [helloword.proto](https://github.com/grpc/grpc-go/blob/master/examples/helloworld/helloworld/helloworld.proto)

3、生成Go源代码，记得加上plugins=grpc否则将无法生成服务端接口
```
$ protoc -I helloworld/ helloworld/helloworld.proto --go_out=plugins=grpc:helloworld
```
### gRPC
代码中需要引用到官方的context包，可在以下链接下载
https://github.com/golang/net
引用刚才我们通过protoc工具生成的代码并分别进行实现。

1、实现服务端代码,参考[greeter_server](https://github.com/grpc/grpc-go/blob/master/examples/helloworld/greeter_server/main.go)    
```

package main

import (
    "log"
    "net"

    pb "your_path_to_gen_pb_dir/helloworld"
    "golang.org/x/net/context"
    "google.golang.org/grpc"
)

// RPC服务端口
const (
    port = ":50051"
)

type server struct{}

// 远程方法的具体实现
func (s *server) SayHello(ctx context.Context, in *pb.HelloRequest) (*pb.HelloReply, error) {
    return &pb.HelloReply{Message: "Hello " + in.Name}, nil
}

func main() {

    // 开启TCP监听
    lis, err := net.Listen("tcp", port)
    if err != nil {
        log.Fatalf("failed to listen: %v", err)
    }
    
    // 实例化gRPC服务器
    s := grpc.NewServer()
    
    // 注册服务
    pb.RegisterGreeterServer(s, &server{})
    s.Serve(lis)
}

```

2、实现客户端代码,参考[greeter_client](https://github.com/grpc/grpc-go/blob/master/examples/helloworld/greeter_client/main.go) 
```
package main

import (
	"log"
	"os"

	"golang.org/x/net/context"
	"google.golang.org/grpc"
	pb "google.golang.org/grpc/examples/helloworld/helloworld"
)

const (
	address     = "localhost:50051"
	defaultName = "world"
)

func main() {
	// 新建Tcp链接
	conn, err := grpc.Dial(address, grpc.WithInsecure())
	if err != nil {
		log.Fatalf("did not connect: %v", err)
	}
	defer conn.Close()
    
    // 根据Protobuf框架，新建客户端实例
	c := pb.NewGreeterClient(conn)

	name := defaultName
	if len(os.Args) > 1 {
		name = os.Args[1]
	}
    
    // 调用远程方法
	r, err := c.SayHello(context.Background(), &pb.HelloRequest{Name: name})
	if err != nil {
		log.Fatalf("could not greet: %v", err)
	}
    
    // 打印响应
	log.Printf("Greeting: %s", r.Message)
}
```

用gRPC写了一个远程请求,发送用户ID可以获得用户名。  
Demo地址:https://github.com/DennisMao/goexperience/tree/master/rpc/GetUser  
Git Clone整个目录下来即可

## 引用

1，《分布式服务框架》李林锋  
2. [《gRPC tutorials》](https://grpc.io/docs/tutorials/basic/go.html)   