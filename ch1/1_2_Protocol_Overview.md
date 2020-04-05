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