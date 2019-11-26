# Netty in action



































































#### Channel的生命周期

Channel接口的状态模型：

| 状态                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| ChannelUnregistered | 已经被创建，但还未注册到EventLoop                            |
| ChannelRegistered   | 已经被注册到了EventLoop                                      |
| Channelactive       | 处于活动状态（已经连接到它的远程节点），它现在已经可以接收和发送数据了 |
| ChannelInactive     | 没有连接到远程节点                                           |

当这些状态发生改变时，将会生成对应的事件。这些事件将会被转发给ChannelPipeline中的ChannelHandler，其随后对他们做出响应。

![channel-status-model](https://raw.githubusercontent.com/szweemedlock/IMarked/master/images/channel-status-model.png)

#### ChannelHandler的生命周期

ChannelHandler接口定义的生命周期操作：

| 类型           | 描述                                              |
| -------------- | ------------------------------------------------- |
| handlerAdded   | 当把ChannelHandler添加到ChannelPipeline中时被调用 |
| handlerRemoved | 当从ChannelPipeline中移除ChannelHandler时被调用   |
| exceptionCaugt | 当处理过程中在ChannelPipeline中有错误产生时被调用 |



> ChannelHandler的两个重要的子接口
>
> * ChannelInboundHandler（处理入站数据以及各种状态变化）
> * ChannelOutboundHandler（处理出站数据并且允许拦截所有的操作）



#### ChannelInboundHandler接口

当某个ChannelInboundHandler的实现重写*channelRead*方法时它将负责显式地释放与池化的ByteBuf实例相关的内存。

![channelInbound](https://raw.githubusercontent.com/szweemedlock/IMarked/master/images/channelInbounderAdapter.png)



Netty将使用*WARN*级别的日志消息记录未释放的资源，但是以这种方式管理资源可能很繁琐。下面是一个更加简单的方式。

![simple-channel](https://raw.githubusercontent.com/szweemedlock/IMarked/master/images/simpleChannelInboundHandler.png)

> 由于SimpleChannelInboundHandler会自动释放资源，所以不应该存储指向任何消息的引用供将来使用，因为这些引用都将会失效。



#### ChannelOutboundHandler接口

它将处理出站操作和数据，Channel、ChannelPipeline和ChannelHandlerContext将调用它的方法。它的一个强大功能是可以按需推迟操作或者事件，这是的可以通过一些复杂的方法来处理请求。*例如，*如果到远程节点的写入被暂停了，那么可以推迟冲刷操作并在稍后继续。



#### ChannelHandler适配器

![channelHandlerAdapter](https://raw.githubusercontent.com/szweemedlock/IMarked/master/images/ChannelHandlerAdapter.png)

> 在ChannelInboundHandlerAdapter和ChannelOutboundAdapter中所提供的方法体调用了其相关联的ChannelHandlerContext上的等效方法，从而将事件转发到了ChannelPipline中的下一个ChannelHandler中。



####  ChannelPipeline接口

它是一个拦截流经Channel的入站和出站事件的ChannelHandler实例链。每一个新建的Channel都将会被分配一个新的ChannelPipeline。这项关联是永久性的。Channel既不能附加另一个ChannelPipeline，也不能分离当前的。在Netty组件的生命周期中，这是一项固定的操作，不需要开发人员的任何干预。



#### ChannelHandlerContext接口



![relate-with-channel](https://raw.githubusercontent.com/szweemedlock/IMarked/master/images/relate-with-channel.png)

