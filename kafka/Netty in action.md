# Netty in action

#### Channel的生命周期

| 状态                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| ChannelUnregistered | 已经被创建，但还未注册到EventLoop                            |
| ChannelRegistered   | 已经被注册到了EventLoop                                      |
| Channelactive       | 处于活动状态（已经连接到它的远程节点），它现在已经可以接收和发送数据了 |
| ChannelInactive     | 没有连接到远程节点                                           |

当这些状态发生改变时，将会生成对应的事件。这些事件将会被转发给ChannelPipeline中的ChannelHandler，其随后对他们做出响应。

![channel-status-model](/Users/songzhewen/Documents/open-source/IMarked/images/channel-status-model.png)

#### ChannelHandler的生命周期

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

