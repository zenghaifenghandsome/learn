# Hive
## 1.什么是Hive
    Hive 是由facebook开源，基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张表，并提供类SQL查询功能。
## 2.Hive的本质
    Hive是一个Hadoop客户端，用于将HQL（Hive SQL）转化成MapReduce程序。
    - Hive中的每张表的数据存储在HDFS
    - Hive分析数据底层的实现是MapReduce（也可配置为Spark或者Tez）
    - 执行程序运行在Yarn上
## Hive架构原理
## metastore服务
 hive和mysql并不是分布式的，在hadoop103上的mysql存储的meta，hadoop102上的hive是无法操作的，这是metastore嵌入式模式，默认嵌入式模式
生产环境不使用嵌入式，使用独立模式
独立模式：hive-cli统一都访问同一个metastore服务（只需要在hive-site.xml 添加metastore服务地址的配置就可以了）



