# Hive
## 1.什么是Hive
    Hive 是由facebook开源，基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张表，并提供类SQL查询功能。
## 2.Hive的本质
    Hive是一个Hadoop客户端，用于将HQL（Hive SQL）转化成MapReduce程序。
    - Hive中的每张表的数据存储在HDFS
    - Hive分析数据底层的实现是MapReduce（也可配置为Spark或者Tez）
    - 执行程序运行在Yarn上
## Hive架构原理
1
## metastore服务
    hive和mysql并不是分布式的，在hadoop103上的mysql存储的meta，hadoop102上的hive是无法操作的，这是metastore嵌入式模式，默认嵌入式模式
    生产环境不使用嵌入式，使用独立模式
    独立模式：hive-cli统一都访问同一个metastore服务（只需要在hive-site.xml 添加metastore服务地址的配置就可以了）
## Hive 启动脚本
    - nohup 放在命令开头 关闭终端进程也继续保持运行状态（不可以ctr c）
    - & 后台运行，不会卡住，可以继续操作终端
    - 2>&1 错误重定向到标准输出
    一般组合使用：nohup [xxx命令操作] >file 2>&1 &  表示将xxx命令运行的结果输出到file中，并保持命令启动的进程在后台运行。

## 文件格式
### textfile
### orc
### Parquet
    结构和orc几乎一致，以页为基本存储单位，Footer中存储了数据的元数据信息，通过footer可以找到具体的数据所在的位置
```sql
create table parquet_tbl(
    id int,
    name string
)
stored as parquest;
```
### 注意：orc和parquet不能直接load数据，必须通过insert方式插入数据
insert into table orc_tbl select id,name from text_file_tbl;



