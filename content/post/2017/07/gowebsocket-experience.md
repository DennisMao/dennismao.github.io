+++
title = "Websocket使用手记"
date = 2017-09-21T23:51:49+08:00
draft = false
slug = ""
categories = ["golang","使用手记"]
tags = ["websocket","golang"]
toc = true
+++


## websocket是什么?
Websocket最起初是在HTTP协议的基础上发展而来，后独立成为了一个标准。其特点是只要建立一次连接，即可保持服务端与客户端的长连接状态，期间可保持全双工的通讯(实时双方互传)。由于省去了重复建立连接产生的性能和带宽消耗，WebSocket可实现高性能的数据实时传递。
## 用于什么场景?
可应用在前后端数据需要实时数据互传的情况，比如：  

+ 聊天系统
+ 股票显示
+ 实时交互游戏

## 为什么要选用它?
选用websocket的理由:
+ 协议的握手建立在http 1.1，因此在开发过程中可以很好的集成到现有的Restful风格中。
+ 协议中的数据传递虽然是websocket的独立协议，但是其传输过程与http请求响应风格相近,开发难度低，学习成本低。
+ 协议的本质还是建立一个socket连接。原生支持wss安全访问，底层支持数据压缩、自动检验和消息包自动拆分。
+ 客户端应用简单，前端对于websocket的处理，代码简单，开发效率高。

## 使用过程
### 服务端
目前Go语言主流的Websocket库有两个：  

+ 官方golang.org/x/websocket 库
+ github.com/gorilla/websocket 库

官方的库目前支持情况不太理想,godoc官方的websocket包页面介绍上推荐使用Gorilla的库。两者对比下，官方的包目前能够实现基本的服务器和客户端连接、收发功能。如果需要快捷开发基本WebSocket功能可使用官方包。需要协议的高级功能的推荐采用Gorilla包。

 
Gorilla/websocket包，丰富了支持Websocket协议内的数据压缩、心跳检测事件、碎片信息。

程序结构 

**websocket**

* main()  
    * 注册普通HTTP路由  
    * 监听HTTP服务  
    * websocket_handler()  
        + Upgrade 升级普通HTTP协议为Websocket协议，建立连接  
        + For循环  
            - ReadMessage 读取信息  
            - WriteMessage 写入信息  

服务端的编写并不复杂，与普通HTTP服务编写相近，先编写一个Handler处理函数，再在主程序中注册路由，开启服务，不同的主要在处理函数中。由于普通HTTP的响应都是无状态，即请求后响应即可退出协程，但Websocket需要协程一直在跑，而且句柄固定，因此我们需要在Websocket的处理函数中加入循环，不让它关闭。

+ 客户端主动断开连接
+ 服务端接收信息失败
+ 服务端发送信息失败
+ 心跳检查失败

### 客户端
#### javascript
参考[websocket.org](http://www.websocket.org/echo.html)的示例代码，  
注意：  

+ 默认的示例代码不支持心跳
+ 默认代码不支持压缩

示例代码：
```
    <!DOCTYPE html>
  <meta charset="utf-8" />
  <title>WebSocket Test</title>
  <script language="javascript" type="text/javascript">

  var wsUri = "ws://echo.websocket.org/"; //填入本机websocket绑定的地址
  var output;

  function init()
  {
    output = document.getElementById("output");
    testWebSocket();
  }

  function testWebSocket()
  {
    websocket = new WebSocket(wsUri);
    websocket.onopen = function(evt) { onOpen(evt) };
    websocket.onclose = function(evt) { onClose(evt) };
    websocket.onmessage = function(evt) { onMessage(evt) };
    websocket.onerror = function(evt) { onError(evt) };
  }

  function onOpen(evt)
  {
    writeToScreen("CONNECTED");
    doSend("WebSocket rocks");
  }

  function onClose(evt)
  {
    writeToScreen("DISCONNECTED");
  }

  function onMessage(evt)
  {
    writeToScreen('<span style="color: blue;">RESPONSE: ' + evt.data+'</span>');
    websocket.close();
  }

  function onError(evt)
  {
    writeToScreen('<span style="color: red;">ERROR:</span> ' + evt.data);
  }

  function doSend(message)
  {
    writeToScreen("SENT: " + message);
    websocket.send(message);
  }

  function writeToScreen(message)
  {
    var pre = document.createElement("p");
    pre.style.wordWrap = "break-word";
    pre.innerHTML = message;
    output.appendChild(pre);
  }

  window.addEventListener("load", init, false);

  </script>

  <h2>WebSocket Test</h2>

  <div id="output"></div>
```
#### golang
