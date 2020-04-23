_本章为非规范章节_

WebSocket 协议使用 Web 浏览器使用的 origin 模型来限制在网页中使用 WebSocket 协议时哪些网页可以连接 WebSocket 服务。自然当客户端使用 WebSocket 协议直连时（例如不在 Web 浏览器中运行的网页），origin 模型就没有用了，因为客户端可以自定义发送任意的 origin 字符串。

该协议无法与现存的如 SMTP [[RFC5321](https://tools.ietf.org/html/rfc5321)] 和 HTTP 服务建立连接，在有需要的情况下 HTTP 服务选择支持该协议。这是通过进行严格而复杂的握手操作以及在握手操作完成之前无法向连接发送数据来实现的（从而限制了许多受影响的服务器）。

同理，其他协议（特别是 HTTP）也无法连接 WebSocket 服务，例如当 HTML “表单”提交数据到 WebSocket 服务时就会发现无法连接。这主要通过要求服务端校验是否已通过握手来实现的，只有服务端在收到特定的握手包（只有 WebSocket 客户端才能发送）时，服务端才能执行此操作。特别是在撰写本规范时，攻击者无法在 Web 浏览器中只通过使用 HTML 和 JS 的 API（例如 [XMLHttpRequest](https://tools.ietf.org/html/rfc6455#ref-XMLHttpRequest)）设置以 `Sec-` 开头的字段。