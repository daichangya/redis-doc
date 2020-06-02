## org.jboss.netty.channel  
Interface ChannelEvent

**All Known Subinterfaces:**

[ChannelStateEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口"), [ChildChannelStateEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChildChannelStateEvent.html "org.jboss.netty.channel中的接口"), [ExceptionEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ExceptionEvent.html "org.jboss.netty.channel中的接口"), [IdleStateEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/handler/timeout/IdleStateEvent.html "org.jboss.netty.handler.timeout中的接口"), [MessageEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/MessageEvent.html "org.jboss.netty.channel中的接口"), [WriteCompletionEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/WriteCompletionEvent.html "org.jboss.netty.channel中的接口")

**All Known Implementing Classes:**

[DefaultChildChannelStateEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/DefaultChildChannelStateEvent.html "org.jboss.netty.channel中的类")，[DefaultExceptionEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/DefaultExceptionEvent.html "org.jboss.netty.channel中的类")，[DefaultIdleStateEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/handler/timeout/DefaultIdleStateEvent.html "org.jboss.netty.handler.timeout中的类")，[DefaultWriteCompletionEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/DefaultWriteCompletionEvent.html "org.jboss.netty.channel中的类")，[DownstreamChannelStateEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/DownstreamChannelStateEvent.html "org.jboss.netty.channel中的类")，[DownstreamMessageEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/DownstreamMessageEvent.html "org.jboss.netty.channel中的类")，[UpstreamChannelStateEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/UpstreamChannelStateEvent.html "org.jboss.netty.channel中的类")，[UpstreamMessageEvent](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/UpstreamMessageEvent.html "org.jboss.netty.channel中的类")

* * *

公共接口**ChannelEvent**

  

![](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelEvent.png)

与关联的I / O事件或I / O请求[`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")。

A [`ChannelEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelEvent.html "org.jboss.netty.channel中的接口")由中的一系列[`ChannelHandler`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelHandler.html "org.jboss.netty.channel中的接口")s 处理[`ChannelPipeline`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelPipeline.html "org.jboss.netty.channel中的接口")。

### Upstream events and downstream events, and their interpretation

每个事件都是上游事件或下游事件。在[`ChannelPipeline`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelPipeline.html "org.jboss.netty.channel中的接口")中如果某个事件从中的第一个处理程序流向最后一个处理程序 ，我们将其称为上游事件，并说**“an event goes upstream”。** 如果某个事件从中的最后一个处理程序流向第一个处理程序，则我们将其称为下游事件，并说**“an event goes downstream.”。** （请参阅图中[`ChannelPipeline`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelPipeline.html "org.jboss.netty.channel中的接口")的更多说明。）

当服务器从客户端接收到消息时，与接收到的消息关联的事件是上游事件。当您的服务器向客户端发送消息或回复时，与写请求关联的事件是下游事件。相同的规则适用于客户端。如果您的客户端向服务器发送了请求，则意味着您的客户端触发了下游事件。如果您的客户端收到服务器的响应，则意味着您的客户端将收到上游事件通知。上游事件往往是入站操作如的结果[`InputStream.read(byte[])`](http://java.sun.com/javase/6/docs/api/java/io/InputStream.html?is-external=true#read(byte[]) "java.io中的类或接口")，和下游事件是对出站的操作，如该请求 [`OutputStream.write(byte[])`](http://java.sun.com/javase/6/docs/api/java/io/OutputStream.html?is-external=true#write(byte[]) "java.io中的类或接口")，[`Socket.connect(SocketAddress)`](http://java.sun.com/javase/6/docs/api/java/net/Socket.html?is-external=true#connect(java.net.SocketAddress) "java.net中的类或接口")和[`Socket.close()`](http://java.sun.com/javase/6/docs/api/java/net/Socket.html?is-external=true#close() "java.net中的类或接口")。

#### Upstream events

| 活动名称 | 事件类型和条件 | 含义 | 警告 |
| --- | --- | --- | --- |
| `"messageReceived"` | [`MessageEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/MessageEvent.html "org.jboss.netty.channel中的接口") | [`ChannelBuffer`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/buffer/ChannelBuffer.html "org.jboss.netty.buffer中的接口")接受到的消息对象（例如） |
| `"exceptionCaught"` | [`ExceptionEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ExceptionEvent.html "org.jboss.netty.channel中的接口") | I / O线程或 [`ChannelHandler`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelHandler.html "org.jboss.netty.channel中的接口") |
| `"channelOpen"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`OPEN`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#OPEN)，value = `true`） | [`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")是开放的，但没有绑定或连接 | **请注意，此事件是从Boss-Thread内部触发的，因此您不应在其中执行任何繁重的操作，因为它将阻止向其他工作人员的调度！** |
| `"channelClosed"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`OPEN`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#OPEN)，value = `false`） |  [`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")已关闭，所有相关资源都已释放 |
| `"channelBound"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`BOUND`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#BOUND)，value = [`SocketAddress`](http://java.sun.com/javase/6/docs/api/java/net/SocketAddress.html?is-external=true "java.net中的类或接口")） |  [`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")已打开并绑定到本地地址，但未连接。 | **请注意，此事件是从Boss-Thread内部触发的，因此您不应在其中执行任何繁重的操作，因为它将阻止向其他工作人员的调度！** |
| `"channelUnbound"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`BOUND`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#BOUND)，value = `null`） | [`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")还没有和local adress绑定 |
| `"channelConnected"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`CONNECTED`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#CONNECTED)，value = [`SocketAddress`](http://java.sun.com/javase/6/docs/api/java/net/SocketAddress.html?is-external=true "java.net中的类或接口")） |  [`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")是打开的，绑定到本地地址，并连接到远程地址 | **请注意，此事件是从Boss-Thread内部触发的，因此您不应在其中执行任何繁重的操作，因为它将阻止向其他工作人员的调度！** |
| `"writeComplete"` | [`WriteCompletionEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/WriteCompletionEvent.html "org.jboss.netty.channel中的接口") | 某些内容已写入远程对等端 |
| `"channelDisconnected"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`CONNECTED`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#CONNECTED)，value = `null`） | [`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")已与其远程对等方断开连接 |
| `"channelInterestChanged"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`INTEREST_OPS`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#INTEREST_OPS)，无值） | [`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")的[`interestOps`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html#getInterestOps())改变 |

这两个附加事件类型仅用于可以具有子频道的父频道（例如[`ServerSocketChannel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/socket/ServerSocketChannel.html "org.jboss.netty.channel.socket中的接口")）。

| 活动名称 | 事件类型和条件 | 含义 |
| --- | --- | --- |
| `"childChannelOpen"` | [`ChildChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChildChannelStateEvent.html "org.jboss.netty.channel中的接口")  （`childChannel.isOpen() = true`） | 一个child [`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")是开放的（例如，服务器通道接受了连接。） |
| `"childChannelClosed"` | [`ChildChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChildChannelStateEvent.html "org.jboss.netty.channel中的接口")  （`childChannel.isOpen() = false`） | 一个child [`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")被关闭了（例如，被接受的连接被关闭了。） |

#### downstream events

| 活动名称 | 事件类型和条件 | 含义 |
| --- | --- | --- |
| `"write"` | [`MessageEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/MessageEvent.html "org.jboss.netty.channel中的接口") | 向发送消息[`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")。 |
| `"bind"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`BOUND`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#BOUND)，value = [`SocketAddress`](http://java.sun.com/javase/6/docs/api/java/net/SocketAddress.html?is-external=true "java.net中的类或接口")） | 绑定[`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")到指定的本地地址。 |
| `"unbind"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`BOUND`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#BOUND)，value = `null`） | [`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")与当前的本地地址解除绑定。 |
| `"connect"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`CONNECTED`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#CONNECTED)，value = [`SocketAddress`](http://java.sun.com/javase/6/docs/api/java/net/SocketAddress.html?is-external=true "java.net中的类或接口")） | 连接[`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")到指定的远程地址。 |
| `"disconnect"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`CONNECTED`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#CONNECTED)，value = `null`） | 断开与[`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")当前远程地址的连接。 |
| `"close"` | [`ChannelStateEvent`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelStateEvent.html "org.jboss.netty.channel中的接口")  （state = [`OPEN`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelState.html#OPEN)，value = `false`） | 关闭[`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")。 |

需要注意的是，这里没有open event，这是因为当一个channel被[`ChannelFactory`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelFactory.html "org.jboss.netty.channel中的接口")创建的话，channel总是已经被打开了。

### 值得阅读的其他资源

请参阅[`ChannelHandler`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelHandler.html "org.jboss.netty.channel中的接口")和[`ChannelPipeline`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelPipeline.html "org.jboss.netty.channel中的接口") 文档，以了解事件如何在管道中流动以及如何在应用程序中处理该事件。


### getChannel

[Channel](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口") **getChannel**（）

返回[`Channel`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/Channel.html "org.jboss.netty.channel中的接口")与此事件相关联的。

* * *

### getFuture

[ChannelFuture ](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelFuture.html "org.jboss.netty.channel中的接口") **getFuture**（）

返回[`ChannelFuture`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/ChannelFuture.html "org.jboss.netty.channel中的接口")与此事件相关联的。如果此事件是上游事件，则此方法将始终返回 [`SucceededChannelFuture`](https://docs.jboss.org/netty/3.2/api/org/jboss/netty/channel/SucceededChannelFuture.html "org.jboss.netty.channel中的类"),因为该事件已经发生。如果此事件是下游事件（即I / O请求），则当I / O请求成功或失败时，将通知返回的Future。
