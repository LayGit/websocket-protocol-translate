_本章为非规范章节_

WebSocket 协议是一个独立基于 TCP 的协议。它和 HTTP 唯一的关系是它的握手是建立在 HTTP 的升级请求上。
默认的情况下，WebSocket 协议使用 `80` 端口进行连接，而基于 TLC（[RFC2818](https://tools.ietf.org/html/rfc2818)）的 WebSocket 连接使用 443 端口进行连接。