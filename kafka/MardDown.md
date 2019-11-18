# Kafka 入门

> Kafka对消息保存时根据Topic进行归类，发送消息成为Peoducer，消息接收者成为COnsumer，此外kafka集群由多个kafka实例组成，每个实例（server）称为broker。



> 主流MQ对比

|               | activemq                          | rabbitmq              | kafka           |
| :-----------: | --------------------------------- | --------------------- | --------------- |
| 所属社区/公司 | apache                            | Mozilla Public Licens | Apache/Linkedln |
|   开发语言    | Java                              | Erlang                | Scala           |
|  支持的协议   | OpenWire、STOMP、REST、XMPP、AMQP | AMQP                  | 仿AMQP          |
|     事务      | 支持                              | 不支持                | 0.11开始支持    |
|     集群      | 支持（不擅长）                    | 支持（不擅长）        | 支持            |
|   负载均衡    | 支持                              | 支持                  | 支持            |
|   动态扩容    | 不支持                            | 不支持                | 支持（zk）      |



> kafka主要特性

* 可发布和订阅流数据，类似于消息队列或者企业级消息系统。
* 以容错的方式存储流数据。



> kafka适合的场景

* 基于Kafka，构造实时流数据管道，让系统或应用之间可靠地获取数据。
* 构建实时流式应用程序，处理流数据或基于数据做出反应。



### 相关概念 - AMQP协议

> AMQP, 是一个提供统一消息服务的标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件而设计。



prouder -push-> broker -pull-> consumer



**server：** AMQP服务端，接受客户端连接，实现AMQP消息队列和路由功能的进程。

**producer：** 生产者，向broker发布消息的客户端应用程序。

**consumer**： 消费者，向消息队列 ？？？



**Topic：** 是数据主题，是kafka中用来代表一个数据流的一个抽象。发布数据时，可用topic对数据进行分类，也作为订阅数据时的主题。一个Topic同时可有多个prouder、consumer。

**Partition：** 每个partition是一个顺序的、不可变的record序列，partition中的record会被分配一个自增长的id，我们称之为offset。

**Replication：** 每个partition还会被复制到其他服务器作为replication 这是一种冗余备份策略。

**Record：**每条记录都有key、value、timestamp三个信息



### kafka核心API

* Producer API *允许一个应用程序发布一串流式的数据到一个或者多个kafka topic。*
* Consumer API *允许一个应用程序订阅一个或多个topic，并且对发布给他们的流式数据进行处理。*
* Streams API *允许一个应用陈旭作为一个流处理器，消费一个或者多个topic产生的输入流，然后生产出一个输出流到一个或多个topic中去，在输入输出流中进行有效的转换。*
* Connector API *允许构建并运行可重用的生产者或者消费者，将Kafka topics连接到以存在的应用程序或者数据系统。比如，李娜街道一个关系型数据可，捕捉表的所有表更内容。*



#### Producer

> * 会为每个partition维护一个缓冲，用来记录还没有发送的数据，每个缓冲区大小用batch.size指定，默认值为16k。
> * linger.ms为，buffer中的数据在达到batch.size前，需要等待的时间
> * acks用来配置请求成功的标准



### Kafka使用场景

#### 消息系统

> 消息系统被用于各种场景，如解耦数据生产者，缓存未处理的消息。kafka可作为传统的消息系统的代替者，与传统消息系统相比，kafka有更好的吞吐量、更好的可用性，这有利于处理大规模的消息。



> 根据经验，通常消息传递对吞吐量要求较低，但可能要求较低的端到端延迟，并经常依赖kafka可靠的durable机制。



> 在这方面，kafka可以与传统的消息传递系统（Actuve MQ和 RabbiteMQ）相媲美。



####存储系统

> 写入到kafka中的数据是落地到磁盘上，并且有冗余备份，kafka允许producer等待确认，通过配置，可实现知道所有的replication完成复制才算写入成功，这样可以保证数据的可用性。



> kafka认真对待存储，并允许client自行控制读取位置，你可以认为kafka是一种特殊的文件系统，它能够提供高性能、低延迟、高可用的日志提交存储。



####日志收集



####流处理



# Kafka Connect数据传输作业工具

> zookeeper是一个开源的分布式组件，**能够提供少量数据的存储，并保证这些数据的一致性**，另外还提供诸如**临时节点自动删除、顺序节点自增长、发布/订阅**等特性，从而能为分布式程序提供协调服务。基于它，可以实现分布式锁、统一命名服务、配置中心、分布式同步等功能。



> zookeeper每次写入数据都是一次事务，所以节点越多，性能越差。



> 增加zookeeper节点是为了提高可用性（融灾），并不是为了提高性能。



> zookeeper一般部署奇数台，提高可用性



HMaster - Zookeeper - regionServers

Zookeeper节点 - 创建节点（znode）- create /P_node "this is a p_node..."

get /p_node



>  kafka里pub一条数据可以消费多次，它会将数据落地到磁盘





