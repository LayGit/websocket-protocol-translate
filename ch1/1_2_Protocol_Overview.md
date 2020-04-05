_本章为非规范章节_

协议包含两个部分：握手和数据传输。

客户端握手包样例：

```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Origin: http://example.com
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
```

服务端握手包样例：

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
Sec-WebSocket-Protocol: chat
```

客户端请求的首行遵循 HTTP 协议请求行规范，服务端请求的首行遵循 HTTP 协议状态行规范。请求行和状态行的规范定义在 [[RFC2616](https://tools.ietf.org/html/rfc2616)]。

在两个样例中，首行下面的几行是一组无序的请求头字段。这些字段的含义包含在本文档的第 4 章节。同时也可以设置其他的请求头字段，例如 cookies [[RFC6265](https://tools.ietf.org/html/rfc6265)]。请求头格式和解析方法定义在 [[RFC2616](https://tools.ietf.org/html/rfc2616)]。

当客户端和服务端发送了各自的握手包，并且握手成功后，就可以开始进行数据传输部分了。这是一个双向的传输通道，两端都可以独立的向对端发送任意数据。

握手成功后，客户端和服务端通过本协议中的“消息”单位进行来回数据传输。在传输中，一条消息由一个消息帧或者多个消息帧组成。WebSocket 消息不对应特定网络层中的帧，因为消息片段可能由中间层合并或拆分。

每个消息帧上有一个类型来表示帧类型，属于同一条消息的每个消息帧包含相同类型的数据。广义的说，它可以是文本数据（可以用 UTF-8 [[RFC3629](https://tools.ietf.org/html/rfc3629)] 编解码的文本 ）、二进制数据（由应用来定义如何解析）或者控制帧（它不是用来传输应用的数据，而是用于协议的信令传输，例如一个关闭连接的信令）。这个版本的协议定义了 6 个帧类型，同时预留了 10 个帧类型以供将来使用。