# kafka 概述
    kafka 是一个分布式的基于发布/订阅模式的消息队列框架

### 发布/订阅模式
    可以理解为生产者和消费者模式，消息的发布者不会直接将消息发送给订阅者，而是将发布的消息分为不同的类型，放到队列中，由订阅者自己去队列中去接收感兴趣的消息

### 消息队列
    企业中常见的消息队列产品：kafka，ActiveMQ，RabbitMQ，RocketMQ
    大数据场景下主要采用kafka
### 消息队列的主要应用场景
    - 缓存/削峰：有助于控制和优化数据流经过系统的速度，解决生产消息和消费消息的处理速度不一致的情况
    - 解耦：允许你独立的扩展或者修改两边的处理过程，只要保证它们遵循同样的接口约束，互不影响。
    - 异步通信：允许用户把一个消息放入队列，但并不立即处理它，然后在需要的时候再去处理它们。
### 消息队列的两种模式
    - 点对点模式：消费者主动拉取消息，消息收到后清除消息
    - 发布/订阅模式：
        + 可以有多个topic主题（如浏览，点赞，收藏，评论等）
        + 消费者消费数据之后不会立刻清楚数据，有框架统一管理删除
        + 每个消费者相互独立，都可以消费到数据
## kafka 基础架构
    - 为方便扩展，并提高高吞吐量，一个topic分为多个partition
    - 配合分区设计，提出消费者组的概念，组内每个消费者并行消费
    - 为提高可用性，为每个partition增加若干个副本，类似NameNode HA
    - 默认依赖zookeeper，zookeeper中记录谁是leader，kafka2.8.0之后可以配置不采用zookeeper，一种全新的模式：Kraft模式
### kafka基础架构组成名词解释
    - Producer：消息生产者，就是向kafka broker发布消息的客户端
    - Consumer：消息消费者，就是从kafka broker取消息的客户端
    - Consumer Group：消费者组，由多个consumer组成,消费组内的每个消费者负责消费不同分区的数据，一个topic下的一个分区只能被一个消费组内的一个消费者消费，消费者之间互不影响，消费者组是逻辑上的一个订阅者
    - Broker：一个kafka服务器就是一个broker,一个broker可以容纳多个topic
    - Topic：可以理解为一个队列，生产者和消费者面向的都是一个topic
    - Partition：为了实现扩展性，一个非常大的topic可以分布到多个broker上，一个topic可以分为多个partition，每个partition都是一个有序的队列
    - Replica：副本，为保证集群中的某个节点发生故障时，该节点上的partition数据不丢失，且kafka任能继续工作，kafka提供了副本机制，一个topic的每个partition都有若干个副本，一个leader和若干个follower
    - leader：每个分区副本的“主”，生产者发送数据的对象，以及消费者消费的对象都是leader
    - follower：每个分区副本的“从”，主动实时的和leader同步数据，在leader发生故障时，成为新的leader
## kafka 快速入门
### 安装部署
    1. 官网下载[kafka官网](http://kafka.apache.org/downloads.html)
    2. 上传到服务器并解压
        tar -zxvf kafka
