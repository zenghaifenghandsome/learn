# kafka 概述
>    kafka 是一个分布式的基于发布/订阅模式的消息队列框架

### 发布/订阅模式
>    可以理解为生产者和消费者模式，消息的发布者不会直接将消息发送给订阅者，
>    而是将发布的消息分为不同的类型，放到队列中，由订阅者自己去队列中去接收感兴趣的消息

### 消息队列
>    企业中常见的消息队列产品：kafka，ActiveMQ，RabbitMQ，RocketMQ
>    大数据场景下主要采用kafka
### 消息队列的主要应用场景
>    - 缓存/削峰：有助于控制和优化数据流经过系统的速度，解决生产消息和消费消息的处理速度不一致的情况
>    - 解耦：允许你独立的扩展或者修改两边的处理过程，只要保证它们遵循同样的接口约束，互不影响。
>    - 异步通信：允许用户把一个消息放入队列，但并不立即处理它，然后在需要的时候再去处理它们。
### 消息队列的两种模式
>    - 点对点模式：消费者主动拉取消息，消息收到后清除消息
>    - 发布/订阅模式：
>        + 可以有多个topic主题（如浏览，点赞，收藏，评论等）
>        + 消费者消费数据之后不会立刻清楚数据，有框架统一管理删除
>        + 每个消费者相互独立，都可以消费到数据
## kafka 基础架构
>    - 为方便扩展，并提高高吞吐量，一个topic分为多个partition
>    - 配合分区设计，提出消费者组的概念，组内每个消费者并行消费
>    - 为提高可用性，为每个partition增加若干个副本，类似NameNode HA
>    - 默认依赖zookeeper，zookeeper中记录谁是leader，
>    kafka2.8.0之后可以配置不采用zookeeper，一种全新的模式：Kraft模式
### kafka基础架构组成名词解释
>    - Producer：消息生产者，就是向kafka broker发布消息的客户端
>    - Consumer：消息消费者，就是从kafka broker取消息的客户端
>    - Consumer Group：消费者组，由多个consumer组成,消费组内的每个消费者负责消费不同分区的数据，
>       一个topic下的一个分区只能被一个消费组内的一个消费者消费，消费者之间互不影响，
>       消费者组是逻辑上的一个订阅者
>    - Broker：一个kafka服务器就是一个broker,一个broker可以容纳多个topic
>    - Topic：可以理解为一个队列，生产者和消费者面向的都是一个topic
>    - Partition：为了实现扩展性，一个非常大的topic可以分布到多个broker上，
>      一个topic可以分为多个partition，每个partition都是一个有序的队列
>    - Replica：副本，为保证集群中的某个节点发生故障时，
>       该节点上的partition数据不丢失，且kafka任能继续工作，kafka提供了副本机制，
>       一个topic的每个partition都有若干个副本，一个leader和若干个follower
>    - leader：每个分区副本的“主”，生产者发送数据的对象，以及消费者消费的对象都是leader
>    - follower：每个分区副本的“从”，主动实时的和leader同步数据，在leader发生故障时，成为新的leader
## kafka 快速入门
### 安装部署
>   1. 官网下载[kafka官网](http://kafka.apache.org/downloads.html)
>   2. 上传到服务器并解压
>       ```shell
>        tar -zxvf kafka 
>        ```
>   3. 修改config目录下的配置文件 **server.properties**
>       ```
>           #broker的全局唯一编号，不能重复只能是数字
>           broker.id=102
>           #处理网络请求的线程数量
>           num.network.threads=3
>           #用来处理磁盘IO的线程数量
>            num.IO.threads=8
>           #发送套接字的缓冲区大小
>           socket.send.buffer.bytes=102400
>           #接收套接字的缓冲区大小
>           socket.receive.buffer.bytes=102400
>           #请求套接字的缓冲区大小
>           socket.request.max.bytes=104857600
>           #kafka运行日志（数据）存放的路径,路径不需要提前创建，kafka自动帮你创建
>           #可以配置多个磁盘路径，路径和路径之间可以用“，”分隔
>           log.dirs=/opt/module/kafka/datas
>           #topic在当前broker上的分区个数
>           num.partitions=1
>           #用来恢复和清理data下数据的线程数
>           num.recovery.threads.per.data.dir=1
>           #每个topic创建时的副本数量，默认是1个副本
>           offsets.topic.replication.factor=1
>           #segment 文件保留的最长时间，超时将被删除
>           log.retention.hours=168
>           #每个segment 文件的大小，默认最大1G
>           log.segment.bytes=1073741824
>           #检查过期数据的时间，默认5分钟检查一次是否数据过期
>           log.retention.check.interval.ms=300000
>           #配置连接Zookeeper集群地址，（在zookeeper根目录创建/kafka,方便管理）
>           zookeeper.connect=hadoop102:2181,hadoop103:2181,hadoop104:2181/kafka
>       ```
>   4.配置环境变量,并分发生效
>   5.分发安装包
>   6.修改配置文件中的broker.id
>   7.启动集群
>       **i.启动zookeeper集群：zk start**
>       **ii.依次在三个节点上启动kafka：bin/kafka-server-start.sh -daemon config/server.properties**
>       **iii.关闭集群：bin/kafka-server-stop.sh**
### kafka群起脚本
#### 脚本编写
```bash
#!/bin/bash
if[ $# == 0 ]
then
    echo -e "请输入参数：\n start: 启动集群\n stop:停止集群\n" && exit
fi
case $1
    "start")
        for host in hadoop102 hadoop103 hadoop104
            do
                echo "==============$host 启动 kafka 集群 ===============
                ssh $host "cd /opt/module/kafka; bin/kafka-server-start.sh -daemon config/server.properties"
            done 
        ;;
    "stop")
        for host in hadoop102 hadoop103 hadoop104
            do
                echo "==============$host 关闭 kafka 集群 ===============
                ssh $host "cd /opt/module/kafka; bin/kafka-server-stop.sh"
            done 
        ;;
    *)
        echo "=============请输入正确的参数================"
        echo -e "start  启动kafka集群;\n stop  停止kafka集群;\n" && exit
        ;;
esac 
``` 
#### 脚本文件添加权限
```linux
sudo chmod 777 kafka
```
#### 注意
> **停止kafka集群时，一定要等kafka集群全部停止完毕后再停止zookeeper集群**
> 因为zookeeper集群中记录者kafka集群的相关信息，zookeeper集群一旦先停止
> kafka集群就无法获取停止进程的信息，只能手动杀死kafka进程
### kafka命令行操作
#### 操作topics命令
>    kafka-topics.sh [参数]
>    1. 查看当前服务器中的所有topic
>       kafka-topics.sh --bootstrap-server hadoop102:9092 --list 
>    2. 创建一个主题名为first的topic
>       kafka-topics.sh --bootstrap-server hadoop102:9092 --create --replication-factor 3 --partitions 1 --topic first
>    3. 查看topic的详情
>      kafka-topics.sh --bootstrap-server hadoop102:9092 --describe --topic first
>    4. 修改分区数 **（注意：分区数只能增加，不能减少）**
>       kafka-topics.sh --bootstrap-server hadoop102:9092 --alter --topic first --partitions 3
>   5.删除topic
>       kafka-topics.sh --bootstrap-server hadoop102:9092 --delete --topic first   
##### 主要参数
| 参数 | 描述 |
|:----:|:----:|
|--bootstrap-server|连接kafka broker主机名称和端口号|
|--topic|操作的topic名称|
|--create|创建主题|
|--delete|删除主题|
|--alter|修改主题|
|--list|查看所有的主题|
|--describe|查看主题详细描述|
|--partitions|设置主题分区数|
|--replication-factor|设置主题分区副本|
|--config|更新系统默认的配置|
#### 生产者命令行操作
##### 命令
```linux
 kafka-console-producer.sh [参数]
```
>   1.生成消息
>       kafka-console-producer.sh --bootstrap-server hadoop102:9092 --topic first
##### 主要参数
|参数|描述|
|:----------:|:--------------:|
|--bootstrap-server|连接kafka broker主机名称和端口号|
|--topic|操作topic的名称|
#### 消费者命令行操作
##### 命令
```linux
    kafka-console-consumer.sh [参数]
```
> 1.消费消息
>   kafka-console-consumer.sh --bootstrap-server hadoop:102 --topic first
> 2.从头开始消费
>   kafka-console-consumer.sh --bootstrap-server hadoop:102 --from-begining --topic first
##### 主要参数
|参数|描述|
|:--:|:--:|
|--bootstrap-server|连接kafka broker主机名称和端口号|
|--topic|操作topic的名称|
|--from-begining|从头开始消费|
|--group|指定消费组名称|
## kafka 生产者
### 生产者消息发送流程
#### 发送原理
> kafka 的 Producer发送消息采用的是**异步发送**的方式
>
> 
