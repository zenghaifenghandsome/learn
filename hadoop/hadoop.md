# Hadoop
## 4大优势
1. 高可靠性：hadoop各个节点存储的数据，有若干个副本，分布在不同的节点上，即使单个节点数据丢失，会从副本节点恢复数据。
2. 高扩展性：hadoop在集群间分配任务数据，可以方便的动态增加节点，无需停止服务。
3. 高效性：hadoop是分布式的集群，其中基于mapReduce的思想，各节点并行工作，处理任务的效率非常高。
4. 高容错性：当因在处理任务中的某个环节失败时，hadoop会自动重新分配任务，保证任务正常进行。

## hadoop的组成
> hadoop的组成分为2个阶段：1.X => 2.X<br>
> 1.X ：
> - mapReduce:兼顾计算和资源调度
> - HDFS：数据存储
> - common：辅助工具<br>
> 2.X:
> - mapReduce:计算
> - Yarn：资源调度
> - HDFS：数据存储
> - common：辅助工具
> 从1.X 到2.X版本，hadoop的组成主要是mapReduce不再完成资源调度工作，另独立出一个模块，Yarn，资源调度都交给Yarn完成。

### HDFS（Hadoop Distributed File System：hadoop 分布式 文件 系统）
HDFS架构：
1. NameNode：储存元数据，如每个文件块的名字，大小，所在的位置等。
2. DataNode：在本地系统存储数据，和数据的校验和。
3. SecondNameNode：每隔一段时间备份NameNode的数据，以此来保证，当NameNode出现故障时，可以及时的顶替工作，和恢复数据。
### Yarn（Yet Another Resource Negotiator：hadoop的资源管理器）
Yarn架构：
1. ResourceManager：统筹管理整个集群的资源。
2. NodeManager：单个集群节点的管理者。
3. Container：封装了执行任务所需的资源，如CPU，内存等，单个任务在container中执行
4. ApplicationManager：任务运行的管理者。
### MapReduce
MapReduce架构：
1. map：并行处理数据
2. reduce：汇总计算结果

## MapReduce HDFS Yarn 的关系
首先客户端提交任务，会发送到Yarn-ResourceManager，ResourceManager会调度资源，找到有资源的节点，启动一个container，运行一个ApplicationManager，ApplicationManager会向ResourceManager申请执行任务所需要的资源，resourceManager分配资源，ApplicationManager拿到资源，启动container，启动mapTask，mapTask从HDFS-DataNode读取数据，执行计算任务，然后启动reduceTask，mapTask计算结果汇总到reduceTask，reduceTask将任务结果写入到DataNode，数据落盘，任务结束，释放资源。

## hadoop的主要目录结构
1. bin：操作hadoop的命令工具
2. sbin：启动关闭hadoop的服务的命令工具
3. share：存放了文档，官方案例，jar包
4. etc：配置文件
5. lib：存放hadoop本地仓库（数据压缩功能的依赖）
## hadoop3种运行模式
1. 本地模式：单机运行，生产中不用
2. 伪分布式：单机运行，模拟分布式，有Hadoop的所有功能，生产不用。
3. 完全分布式：多台服务器组成的集群。

### 本地模式（官方案例wordCount启动流程）
1. 创建输入数据的文件夹：mkdir wcinput
2. 创建数据文件：touch word.txt
3. 创建数据：vim word.txt
4. 启动：hadoop jar  share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.4.jar wordcount wcinput wcoutput
5. 查看结果： cat wcoutput/part-r-00000

### 完全分布式
1. rsync 文件分发工具 
```rsync -av $pdir/$fname $user@$host:$pdir/$fname ```
2. xsync 集群分发脚本
   ```bash
   #!/bin/bash
   # 判断参数个数
   if [ $# -lt 1 ]
   then 
        echo "请输入参数"
        exit;
    fi
    
    #遍历集群
    for host in hadoop102 hadoop103 hadoop104
    do
        echo "=================$host========================="
        #遍历目录
        for file in $@
        do
            #判断文件是否存在
            if [ -e $file]
            then 
                #获取父目录
                pdir=$(cd -P $(dirname $file); pwd)

                #获取当前文件的名字
                fname=$(basename $file)

                #连接远程服务器，创建文件夹
                ssh $host "mkdir -p $pdir"
                # 分发文件
                rsync -av $pdir/$fname $host:$pdir
            else 
                echo "文件不存在"
            fi
        done
    done
   ```
## 集群部署规划
|节点|Hadoop102|Hadoop103|hadoop104|
|-|-|-|-|
|HDFS|DataNode <br> NameNode|DataNode|DataNode<br>SecondNameNode|
|YARN|NodeManager|NodeManager<br>ResourceManager|NodeManager|

## 集群配置文件
### 核心配置文件---[core-site.xml]
```xml
<configuration>
    <!--指定NameNode地址 -->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://hadoop102:8020</value>
    </property>
    
    <!-- 指定hadoop数据的储存目录-->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/module/hadoop/data</value>
    </property>

    <!-- 配置HDFS网页登录使用的静态用户为atguigu-->
    <property>
        <name>hadoop.http.staticuser.user</name>
        <value>atguigu</value>
    </property>
</configuration>
```
### HDFS配置文件---[hdfs-site.xml]
```xml
<configuration>
    <!--NameNode 网页访问地址-->
    <property>
        <name>dfs.namenode.http-address</name>
        <value>hadoop102:9870</value>
    </property>

    <!-- SecondNameNode 网页访问地址-->
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>hadoop104:9868</value>
    </property>
</configuration>
```
### yarn配置文件---[yarn-site.xml]
```xml
<configuration>
    <!-- 指定mapReduce走shuffle-->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>

    <!-- 指定ResourceManager的节点地址-->
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>hadoop103</value>
    </property>

    <!-- 环境变量的继承-->
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>
        JAVA_HOME,
        HADOOP_COMMON_HOME,
        HADOOP_HDFS_HOME,
        HADOOP_CONF_DIR,
        CLASSPATH_PREPEND_DISTCACHE,
        HADOOP_YARN_HOME,
        HADOOP_MAPRED_HOME
        </value>
    </property>
</configuration>
```
### MapReduce配置文件---[mapred-site.xml]
```xml
<configuration>
    <!--指定mapReduce程序在yarn上运行-->
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
<configuration>
```
### 群起集群配置workers文件
hadoop/etc/hadoop/workers

>hadoop102<br>
>hadoop103<br>
>hadoop104<br>

<font color='yellow' size='9'>注意：文件中不要有空行，行尾不要有空格</font>




