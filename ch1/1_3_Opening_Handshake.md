_本章为非规范章节_

打开阶段握手是为了兼容基于 HTTP 的服务器端软件和中间件，以便一个端口可以同时用于 HTTP 客户端和 WebSocket 客户端与服务端之间的交互。最后，WebSocket 客户端的握手包是一个基于 HTTP 协议的升级请求：

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
按照 [[RFC2616](https://tools.ietf.org/html/rfc2616)] 中的约定，握手包中的请求头字段可以是无序的，所以收到不同的请求头顺序是无关紧要的。

WebSocket 连接的地址由一个支持 GET 方法请求的 URI [[RFC2616](https://tools.ietf.org/html/rfc2616)] 来定义，既允许一个 IP 地址服务多个域名，也允许单个服务器服务多个 WebSocket 地址。

客户端通过在握手包中设置 Host 请求头字段来传输主机名，这样客户端和服务端都能知道是与哪个主机建立了连接。剩下的那些请求头字段用于 WebSocket 协议中的定义字段。这个版本定义的字段有：子协议选择（Sec-WebSocket-Protocol）、客户端扩展列表（Sec-WebSocket-Extensions）、Origin 字段等等。`Sec-WebSocket-Protocol` 请求头字段可以用来表示客户端支持哪些子协议（在 WebSocket 协议层之上的应用层协议）。服务端从可以从中选择一个协议或者不选择任何协议，并且在响应握手包中告诉客户端自己的选择。

```
Sec-WebSocket-Protocol: chat
```

Origin 字段 [[RFC6454](https://tools.ietf.org/html/rfc6454)] 用于保护 WebSocket 服务端免受那些未授权的在 Web 浏览器中运行的 WebSocket API 脚本的跨域攻击。服务端能够获悉生成 WebSocket 连接请求的脚本域来源，如果服务端不想接收此来源的请求，可以通过发送一个 HTTP 错误码来拒绝该连接。这个字段由客户端发送，对于非浏览器客户端，如果它在客户端环境中有意义，也可以发送。

最后，服务端要给客户端证明自己收到了客户端的 WebSocket 握手包，以便拒绝那些不是 WebSocket 的连接。这可以防范那些通过 [XMLHttpRequest](https://tools.ietf.org/html/rfc6455#ref-XMLHttpRequest) 或者表单发出的高仿包攻击的攻击者。

为了证明收到了握手包，服务端必须回复一个由两部分信息组成的响应包。第一部分信息来自客户端请求头中的 `Sec-WebSocket-Key` 字段：

```
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
```

对于这个字段，服务端需要取出它的值（它在请求头字段中，例如是一串去除两端空格并经过 Base64 [[RFC4648](https://tools.ietf.org/html/rfc4648)] 编码的字符串），然后用这个字符串与一个固定的 GUID（[RFC4112](https://tools.ietf.org/html/rfc4122)）“`258EAFA5-E914-47DA-95CA-C5AB0DC85B11`” 拼接起来，它不太可能被不理解 WebSocket 协议的网络节点使用。最后使用 SHA-1 算法（160 位）[[FIPS.180-3](https://tools.ietf.org/html/rfc6455#ref-FIPS.180-3)] 和 Base64 编码（详见 [[RFC4648](https://tools.ietf.org/html/rfc4648#section-4)] 第 4 章）对拼接的字符串进行计算，并最终在服务端的我握手包中返回。

具体而言，如果在上面例子中，`Sec-WebSocket-Key` 字段的值为 “`dGhlIHNhbXBsZSBub25jZQ==`”，服务端将此值拼上字符串“`258EAFA5-E914-47DA-95CA-C5AB0DC85B11`” 得到 “`dGhlIHNhbXBsZSBub25jZQ==258EAFA5-E914-47DA-95CA-C5AB0DC85B11`”。接着使用 SHA-1 算法对字符串进行散列计算，得到值 `0xb3 0x7a 0x4f 0x2c 0xc0 0x62 0x4f 0x16 0x90 0xf6 0x46 0x06 0xcf 0x38 0x59 0x45 0xb2 0xbe 0xc4 0xea`。然后用 Base64 [[RFC4648](https://tools.ietf.org/html/rfc4648)] 对这个值进行字符串编码，得到字符串 “`s3pPLMBiTxaQ9kYGzzhZRbK+xOo=`”。把这个字符串放入响应包的 `Sec-WebSocket-Accept` 头字段中返回。

服务端的握手响应包要比客户端的更简单一些。第一行是 HTTP 状态行，状态码固定为 101：

```
HTTP/1.1 101 Switching Protocols
```

任何 101 以外的状态码表示 WebSocket 没有正常完成且 HTTP 协议中的状态码在这里同样适用。请求头跟随状态码。
响应头中的 `Connection` 和 `Upgrade` 字段表示完成 HTTP 请求升级。`Sec-WebSocket-Accept` 字段表示服务端是否接受连接。如果该值存在，这个值必须是一个由客户端发送的 `Sec-WebSocket-Key` 值与固定的 GUID 哈希计算后的值。任何其他的值都表示服务端拒绝连接。

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```
这些字段由 WebSocket 客户端的相关脚本页进行检查，如果 `Sec-WebSocket-Accept` 值与期望的值不匹配或者请求头字段缺失，又或者 HTTP 状态码不是 101，则 WebSocket 连接不会被建立，数据帧也不会被发送。

可选的一些字段也可以被包含在请求头中。在这个版本的协议中，主要的可选字段是 `Sec-WebSocket-Protocol`，它表示服务端选择了哪个子协议。WebSocket 客户端将验证服务端从自己在请求握手包中发送的子协议中的哪个。服务端只能从客户端握手包发送的子协议选项中选择，并且在自己的响应包中声明选择了哪个。

```
Sec-WebSocket-Protocol: chat
```

服务端也可以设置 cookie 相关的字段来设置 cookies，具体描述参考 [[RFC6265](https://tools.ietf.org/html/rfc6265)]