_本章为非规范章节_

客户端可以它的握手请求中包含 `Sec-WebSocket-Protocol` 字段来指定要使用的子协议。如果客户端指定了这个字段，服务端需要在连接响应中包含同样的字段来选择一个子协议。

这些子协议的名称应该按照 11.5 章节中的规范进行注册。为了避免潜在的冲突，建议使用包含 ASCII 版本的域名名称作为子协议的名称。举个例子，如果某公司创建了一个被许多 Web 站点服务器实现的聊天子协议，命名为 “chat.example.com”，某组织的子协议是 “chat.example.org”，则两个子协议可以由服务端同时实现，服务端会根据客户端发送的值动态选择要使用的子协议。

子协议可以通过改变协议名称中的版本号来控制协议的向后兼容性，例如将 “bookings.example.net” 改为 “v2.bookings.example.net”。WebSocket 客户端将把这些子协议当做互相独立的子协议处理。向后兼容的协议版本可以通过复用同一子协议字符串实现，但要仔细设计实际的子协议以支持这种可扩展性。