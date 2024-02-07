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

## WebSocket

#### WebSocket 是基于TCP的一种新的应用层网络协议，提供全双工通信的协议。它允许在客户端和服务器之间建立持久连接，以便实时双向通信。因此，在 WebSocket 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输，客户端和服务器之间的数据交换变得更加简单

1. **实时通信：** WebSocket 提供了实时的双向通信，允许服务器主动向客户端推送数据，而不需要客户端不断地发起请求。
2. **低延迟：** 由于 WebSocket 使用单个持久连接，可以减少通信的延迟和带宽消耗。
3. **协议独立：** WebSocket 是一种独立的协议，与 HTTP/HTTPS 协议无关。它通常在标准 HTTP 或 HTTPS 协议的基础上升级而来。
4. **更灵活的通信方式**： HTTP 请求和响应通常是一一对应的，而 WebSocket 允许服务器和客户端之间以多种方式进行通信，例如消息Push, 事件推送等。

缺点：

1. 不支持无连接：WebSocket 是一种持久化的协议，这意味着连接不会在一次请求之后立即断开。这是有利的，因为它消除了建立连接的开销，但也可能导致一些资源泄漏的问题
2. 需要特殊的服务器支持：WebSocket 需要服务器端支持，只有特定的服务器才能够实现 WebSocket 协议。

#### HTTPS 与 WebSocket 一起使用：

在实际的 Web 应用程序中，HTTPS 和 WebSocket 可以结合使用，提供安全、实时的通信。在这种情况下，通常的做法是在 HTTPS 连接上建立 WebSocket 连接，确保数据的传输是受到保护的。

1. **使用安全的 WebSocket 协议（wss）：** 当应用程序已经使用 HTTPS 时，WebSocket 连接应使用安全的 WebSocket 协议（wss://）而不是普通的 WebSocket 协议（ws://）。
2. **相同的域和端口：** 为了确保安全性，WebSocket 连接通常限制在与应用程序相同的域和端口。

使用安全 WebSocket 连接的例子：

```
const socket = new WebSocket('wss://example.com/socket');
```

需要确保服务器端已经配置支持安全 WebSocket 连接。在一些情况下，WebSocket 连接可能会通过与应用程序相同的 HTTPS 服务器端点提供。

### WebSocket 工作原理

1. 握手阶段

WebSocket 在建立连接时需要进行握手阶段。握手阶段包括以下几个步骤：

* 客户端向服务器发送请求，请求建立 WebSocket 连接。请求中包含一个 Sec-WebSocket-Key 参数，用于生成WebSocket的随机密钥。
* 服务端接收到请求后，生成一个随机密钥，并使用随机密钥生成一个新的 Sec-WebSocket-Accept 参数
* 客户端收到Sec-WebSocket-Accept 后，使用原来的随机密钥和新的Sec-WebSocket-Accept参数共同生成一个新的 Sec-WebSocket-Key参数，用于加密数据传输。
* 客户端将新的Sec-WebSocket-Key参数发送给服务器端，服务器端收到后，使用该参数加密数据传输。

2. 数据传输阶段

建立连接后，客户端和服务端就可以通过WebSocket进行实时双向通信。数据传输阶段包括以下几个步骤：

* 客户端向服务端发送数据，服务端收到数据后将其转发给其他客户端。
* 服务端向客户端发送数据，客户端收到数据后进行处理。

**双方如何进行相互传输数据的** 具体的数据格式是怎么样的呢？WebSocket 的每条消息可能会被切分成多个数据帧（最小单位）。发送端会将消息切割成多个帧发送给接收端，接收端接收消息帧，并将关联的帧重新组装成完整的消息。

发送方 -> 接收方：ping。

接收方 -> 发送方：pong。

ping 、pong 的操作，对应的是 WebSocket 的两个控制帧

3. 关闭阶段

当不再需要WebSocket连接时，需要进行关闭阶段。关闭阶段包括以下几个步骤：

* 客户端向服务端发送关闭请求，请求中包含一个WebSocket的随机密钥。
* 服务端接收到关闭请求后，向客户端发送关闭响应，关闭响应中包含服务端生成的随机密钥。
* 客户端收到关闭响应后，关闭WebSocket连接。

总的来说，WebSocket通过握手阶段、数据传输阶段和关闭阶段实现了服务器和客户端之间的实时双向通信

### JavaScript 中 WebSocket 对象的属性和方法，以及如何创建和连接 WebSocket。

WebSocket 对象的属性和方法：

1. `WebSocket` 对象：WebSocket 对象表示一个新的 WebSocket 连接。
2. `WebSocket.onopen` 事件处理程序：当 WebSocket 连接打开时触发。
3. `WebSocket.onmessage` 事件处理程序：当接收到来自 WebSocket 的消息时触发。
4. `WebSocket.onerror` 事件处理程序：当 WebSocket 发生错误时触发。
5. `WebSocket.onclose` 事件处理程序：当 WebSocket 连接关闭时触发。
6. `WebSocket.send` 方法：向 WebSocket 发送数据。
7. `WebSocket.close` 方法：关闭 WebSocket 连接。

创建和连接 WebSocket：

1. 创建 WebSocket 对象：

```
var socket = new WebSocket('ws://example.com');
```

其中，`ws://example.com` 是 WebSocket 的 URL，表示要连接的服务器。

2. 连接 WebSocket：

使用 `WebSocket.onopen` 事件处理程序检查 WebSocket 是否成功连接。

```
socket.onopen = function() {
    console.log('WebSocket connected');
};
```

3. 接收来自 WebSocket 的消息：

使用 `WebSocket.onmessage` 事件处理程序接收来自 WebSocket 的消息。

```
socket.onmessage = function(event) {
    console.log('WebSocket message:', event.data);
};
```

4. 向 WebSocket 发送消息：

使用 `WebSocket.send` 方法向 WebSocket 发送消息。

```
socket.send('Hello, WebSocket!');
```

5. 关闭 WebSocket：

当需要关闭 WebSocket 时，使用 `WebSocket.close` 方法。

```
socket.close();
```

注意：在 WebSocket 连接成功打开和关闭时，会分别触发 `WebSocket.onopen` 和 `WebSocket.onclose` 事件。在接收到来自 WebSocket 的消息时，会触发 `WebSocket.onmessage` 事件。当 WebSocket 发生错误时，会触发 `WebSocket.onerror` 事件。

### 五.webSocket应用场景

1. 实时通信：WebSocket 非常适合实时通信场景，例如聊天室、在线游戏、实时数据传输等。通过 WebSocket，客户端和服务器之间可以实时通信，无需依赖轮询，从而提高通信效率和减少网络延迟。
2. 监控数据传输：WebSocket 可以在监控系统中实现实时数据传输，例如通过 WebSocket，客户端可以实时接收和处理监控数据，而无需等待轮询数据。
3. 自动化控制：WebSocket 可以在自动化系统中实现远程控制，例如通过 WebSocket，客户端可以远程控制设备或系统，而无需直接操作。
4. 数据分析：WebSocket 可以在数据分析场景中实现实时数据传输和处理，例如通过 WebSocket，客户端可以实时接收和处理数据，而无需等待数据存储和分析。
5. 人工智能：WebSocket 可以在人工智能场景中实现实时数据传输和处理，例如通过 WebSocket，客户端可以实时接收和处理数据，而无需等待数据处理和分析。















