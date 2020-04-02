## 摘要
WebSocket 协议允许在一个受控环境中运行不受信任代码的客户端与选择从该代码进行通信的远程主机之间进行双向通信。协议的安全模型复用了一般在 Web 浏览器中使用的基于 origin 的安全模型。 WebSocket 协议构建于 TCP 协议层之上，由一个握手协议和基本消息帧组成。这项技术的目标是为那些基于浏览器的应用提供一种能够与服务端进行双向通信的机制，而不需要依赖于打开多个 HTTP 连接。（例如使用 XMLHttpRequest 或者 iframe 和长轮询）

## 备忘录状态
这是一个互联网标准的跟踪文档。

这个文档是由互联网工程任务组（IETF）制定的，它代表了互联网工程任务组社区的共识。本文档已经接受过公众的审查，并已通过互联网工程指导小组（IESG）的审核。有关互联网标准的更多信息，请参阅 RFC  5741 第 2 节。

## 版权说明
Copyright (c) 2011 IETF Trust and the persons identified as the document authors. All rights reserved.

本文档受 BCP 78 和 IETF Trust 有关 IETF 文件的法律规定（[http://trustee.IETF.org/license-info](http://trustee.IETF.org/license-info)）的约束，该法律规定在本文件发布之日起生效。请仔细阅读这些文件，因为他们描述了您对本文档的权利和限制。从本文档提取的代码组件必须包含如第 4 节所述的法律规定的包含简化的 BSD 许可证文本。