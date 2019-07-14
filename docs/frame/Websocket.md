# 第六章 Websocket
## 1 长轮询
在网页，我们经常扫码登录，结合之前的学习的知识点，来思考下，前端是如何知道用户在手机上扫码登录了呢？
![](img/Websocket)
### 1.1 长轮询
客户端不断的向服务器发送请求
### 1.2 缺点
1. 开销大
2. 浪费资源
3. 消耗流量

### 1.3 长轮询总结
1. 长轮询: 长轮询的基本概念

## 2 Websocket介绍
长轮询消耗太多资源，其中主要原因是客户端和服务端并没有一直连接在一起，如果能够让客户端和服务器一直保持连接呢？

### 2.1 正经介绍
WebSocket 协议是基于 TCP 的一种新的 HTML5 网络协议。它实现了浏览器与服务器全双工(full-duplex)通信——允许服务器主动发送信息给客户端。WebSocket通信协议于2011年被IETF定为标准RFC 6455，并被RFC7936所补充规范

### 2.2 简单说
客户端和服务器一直连接在一起

### 2.3 Websocket总结
1. Websocket: websocket基本概念

## 3 Websocket服务端编程
刚才介绍了websocket，那么该如何使用呢？

### 3.1 导入websocket
```
import tornado.websocket
from tornado.websocket import WebSocketHandler
```

### 3.2 基类
```

```

## 4 Websocket客户端编程