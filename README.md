# 🐽 HTTP(S) 协议 与 WebSocket

HTTP（Hypertext Transfer Protocol）和HTTPS（Hypertext Transfer Protocol Secure）是用于在网络上传输数据的协议。这两者都是应用层协议，用于客户端和服务器之间的通信。以下是它们的主要区别：

#### HTTP (Hypertext Transfer Protocol):

1. **不安全性：** HTTP 是不安全的协议，传输的数据是明文的，容易被拦截和窃取。因此，不建议在敏感场景中使用纯HTTP。
2. **端口：** 默认使用端口80。
3. **速度：** 由于不涉及加密，HTTP 的传输速度可能会稍微快一些。
4. **URL：** HTTP 的 URL 以 "http://" 开头。

#### HTTPS (Hypertext Transfer Protocol Secure):

1. **安全性：** HTTPS 使用了 SSL/TLS 协议进行加密，因此传输的数据是加密的，更加安全。这对于涉及用户敏感信息的网站（例如登录信息、支付信息等）至关重要。
2. **端口：** 默认使用端口443。
3. **速度：** 由于涉及加密和解密过程，HTTPS 的传输速度可能会略微减慢。
4. **URL：** HTTPS 的 URL 以 "https://" 开头。

#### 共同点：

1. **协议：** 两者都是基于请求-响应模型的协议，用于在客户端和服务器之间传输超文本。
2. **使用场景：** 通常用于浏览器与服务器之间的通信，用于获取和传输网页上的信息。

#### WebSocket

#### WebSocket 是一种在单个TCP连接上提供全双工通信的协议。它允许在客户端和服务器之间建立持久连接，以便实时双向通信。

1. **实时通信：** WebSocket 提供了实时的双向通信，允许服务器主动向客户端推送数据，而不需要客户端不断地发起请求。
2. **低延迟：** 由于 WebSocket 使用单个持久连接，可以减少通信的延迟和带宽消耗。
3. **协议独立：** WebSocket 是一种独立的协议，与 HTTP/HTTPS 协议无关。它通常在标准 HTTP 或 HTTPS 协议的基础上升级而来。

#### HTTPS 与 WebSocket 一起使用：

在实际的 Web 应用程序中，HTTPS 和 WebSocket 可以结合使用，提供安全、实时的通信。在这种情况下，通常的做法是在 HTTPS 连接上建立 WebSocket 连接，确保数据的传输是受到保护的。

1. **使用安全的 WebSocket 协议（wss）：** 当应用程序已经使用 HTTPS 时，WebSocket 连接应使用安全的 WebSocket 协议（wss://）而不是普通的 WebSocket 协议（ws://）。
2. **相同的域和端口：** 为了确保安全性，WebSocket 连接通常限制在与应用程序相同的域和端口。

使用安全 WebSocket 连接的例子：

```
const socket = new WebSocket('wss://example.com/socket');
```

需要确保服务器端已经配置支持安全 WebSocket 连接。在一些情况下，WebSocket 连接可能会通过与应用程序相同的 HTTPS 服务器端点提供。
