---
layout:     post
title:      SpringBoot使用WebSocket
subtitle:   服务器通信
date:       2020-12-06
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - WebSocket
---

## 导入依赖
```` 
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
````
## Server编写

首先编写一个Server 类：

```` java
public class WebSocketServer {
}
````

### 添加注解方法
为该类添加注解以应用

```` java 
@Component
@ServerEndpoint("/front/websocket/{userId}")
@Slf4j
public class WebSocketServer {
}
````

- @Component : `org.springframework.stereotype.Component` : 将此类声明为组件并交由Spring 管理  
- @ServerEndpoint : `javax.websocket.server.ServerEndpoint` : 声明为WebSocket 服务类; `value` 为客户端注册的URI
- @Slf4j : `lombok.extern.slf4j` : 和本文主题``无关，用于日志输出  


### 实现方法
打开第三方的websocket 依赖，我们会发现，在 `javax.websocket`下有4个注解：`@OnOpen`、`@OnMessage`、`@Close`、`@Error`    


我们需要在 Server 类中新加方法并使用到这些注解

#### @OnOpen
我们新增一个OnOpen方法

```` java
   @OnOpen
    public void onOpen() {

    }
````
我们定义了一个方法，这样在一个客户端建立链接时会执行此方法。  

一般情况下，我们使用 websocket 时会需要向指定的会员发送信息。  

所以我们为添加两个参数：
 * javax.websocket.Session  
 * String

 ````  java
 	@OnOpen
    public void onOpen(Session session, String userId) {

    }
 ````  
 只定义不使用可不行，session 指的是每一个不同的链接，为了给指定用户发送消息，我们需要保存所有的session  

 在这里，我们使用类变量  ConcurrentHashMap 来保存每一个不同的session:

 为了能够识别，同时增加类变量userId :   

 ```` java
public class WebSocketServer {
    /**
     * 所有链接对象
     */
    private static ConcurrentHashMap<String,Session> webSocketMap = new ConcurrentHashMap<>();
    /**
     * 用户ID
     */
    private String userId;


    /**
     * 连接建立成功调用的方法
     */
    @OnOpen
    public void onOpen(Session session, @PathParam("userId") String userId) {
        // 保存userId 到类变量
        this.userId = userId;
        // 连接建立：存入map
        webSocketMap.put(userId, session);
        log.info("WebSocketServer -> onOpen -> new connect -> userId -> {}", userId);
    }
}
 ````
细心的同学已经发现了一个新注解： `@PathParam`，这是用于获取注册URI中的路径参数`{userId}`而使用的。  

类包：  javax.websocket.server.PathParam  

#### @OnMessage
WebSocket 是双向链接： 服务器可以给客户端发消息，客户端当然也可以给服务器发消息。  

我们定义一个方法用来处理当客户端主动给服务器发送消息的情况。  

```` java
	/**
     * 收到客户端消息的处理
     *
     * @param message 消息内容
     */
    @OnMessage
    public void onMessage(String message) {
        log.info("WebSocketServer -> onMessage -> received ->  userId -> {} message -> {}",userId,message);
    }
````  

此处只做了日志打印，需要的小伙伴可以自己实现所需要的业务逻辑  

#### @OnClose
有建立链接，自然就有关闭链接

```` java  
	/**
     * 连接关闭调用的方法
     */
    @OnClose
    public void onClose() {
        // 链接关闭，移除map中的保存信息
        webSocketMap.remove(userId);
        log.info("WebSocketServer -> onClose -> userId -> {}", userId);
    }
````
#### @OnError  
作为一个健壮的程序，当然省不了各种的异常处理了。  

我们使用 @OnError 来处理错误：  

```` java
    /**
     * 错误处理
     */
    @OnError
    public void onError(Session session, Throwable e) {
        log.error("WebSocketServer -> onError", e);
    }

````  

同样，此处只做了日志打印

### 向客户端发送消息
websocket 主要特性之一 就是发送消息到客户端，发送消息主要是通过调用 Session 中的方法。  

```` java
	/**
     * 发送自定义消息
     */
    public static void send(String message,String userId) throws IOException {
        log.info("WebSocketServer -> send -> userId -> {} msg -> {}",userId,message);
        if (StrUtil.isNotBlank(userId) && webSocketMap.containsKey(userId)) {
            webSocketMap.get(userId).getBasicRemote().sendText(message);
        } else {
           log.warn("WebSocketServer -> send -> offline -> userId -> {}",userId);
        }
    }
````  

这个方法是静态方法：我们可以在需要的位置直接调用此方法来向指定的用户发送信息：

```` java 
WebSocketServer.send("message content","1");
````

## 完整的Java 代码
```` java
/**
 * @author Autuan.Yu
 */
@ServerEndpoint("/front/websocket/{userId}")
@Component
@Slf4j
public class WebSocketServer {
    /**
     * 所有链接对象
     */
    private static ConcurrentHashMap<String, Session> webSocketMap = new ConcurrentHashMap<>();
    /**
     * 用户ID
     */
    private String userId;

    /**
     * 连接建立成功调用的方法
     */
    @OnOpen
    public void onOpen(Session session, @PathParam("userId") String userId) {
        // 保存userId 到类变量
        this.userId = userId;
        // 连接建立：存入map
        webSocketMap.put(userId, session);
        log.info("WebSocketServer -> onOpen -> new connect -> userId -> {}", userId);
    }

    /**
     * 收到客户端消息的处理
     *
     * @param message 消息内容
     */
    @OnMessage
    public void onMessage(String message) {
        log.info("WebSocketServer -> onMessage -> received ->  userId -> {} message -> {}",userId,message);
    }


    /**
     * 连接关闭调用的方法
     */
    @OnClose
    public void onClose() {
        // 链接关闭，移除map中的保存信息
        webSocketMap.remove(userId);
        log.info("WebSocketServer -> onClose -> userId -> {}", userId);
    }

    /**
     * 错误处理
     */
    @OnError
    public void onError(Session session, Throwable e) {
        log.error("WebSocketServer -> onError", e);
    }

    /**
     * 发送自定义消息
     */
    public static void send(String message,String userId) throws IOException {
        log.info("WebSocketServer -> send -> userId -> {} msg -> {}",userId,message);
        if (StrUtil.isNotBlank(userId) && webSocketMap.containsKey(userId)) {
            webSocketMap.get(userId).getBasicRemote().sendText(message);
        } else {
            log.warn("WebSocketServer -> send -> offline -> userId -> {}",userId);
        }
    }
}
```` 
## HTML 
### 启动Server 
到这一步，服务器端就编写完成了，我们先通过RUN 将我们的WebSocket服务端启动, 这一步便不细说了。  
### 新建HTML文件  
在桌面，或者任何其他位置，新建一个HTML 文件。 文件内容则是普普通通的标准HTML 格式： 
```` html 
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>websocket</title>
</head>
<body></body>
```` 

### 增加body 内容
<body>
	<p>userId：<div><input id="userId" name="userId" type="text" value="1"></div>
	<div><a onclick="openSocket()">开启socket</a></div>
</body>

### script 编写
#### 引入 jquery
为了快速编写，先引入jquery,这里直接引入远程的jquery
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
#### 编写websocket支持
事实上，现在主流的浏览器都内置了WebSocket的支持，我们可以直接通过`WebSocket`对象来使用
```` html
<script>
function openSocket() {
    let socket;
        if(typeof(WebSocket) == "undefined") {
            console.info("浏览器不支持WebSocket");
        }else{
            let socketUrl="ws://localhost:8081/front/websocket/"+$("#userId").val();
            if(socket!=null){
                socket.close(); 
                socket=null;
            } 
            socket = new WebSocket(socketUrl);
            //打开事件  
            socket.onopen = function() {
                console.info("websocket已打开"); 
            };
            //获得消息事件
            socket.onmessage = function(msg) {
                console.info(msg.data);
            };
            //关闭事件
            socket.onclose = function() {
                console.info("websocket已关闭");
            };
            //发生了错误事件
            socket.onerror = function() {
                console.info("websocket发生了错误");
            }
        }
    }
</script>
````
## 完整的 HTML 代码

```` html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>websocket</title>
</head>
<body>
    <p>userId：<div><input id="userId" name="userId" type="text" value="1"></div>
    <div><a onclick="openSocket()">开启socket</a></div>
</body>
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
<script>
function openSocket() {
    let socket;
        if(typeof(WebSocket) == "undefined") {
            console.info("浏览器不支持WebSocket");
        }else{
            let socketUrl="ws://localhost:8081/front/websocket/"+$("#userId").val();
            if(socket!=null){
                socket.close(); 
                socket=null;
            } 
            socket = new WebSocket(socketUrl);
            //打开事件  
            socket.onopen = function() {
                console.info("websocket已打开"); 
            };
            //获得消息事件
            socket.onmessage = function(msg) {
                console.info(msg.data);
            };
            //关闭事件
            socket.onclose = function() {
                console.info("websocket已关闭");
            };
            //发生了错误事件
            socket.onerror = function() {
                console.info("websocket发生了错误");
            }
        }
    }
</script>
```` 
