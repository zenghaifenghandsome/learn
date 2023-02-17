# Hadoop
## 4大优势
1. 高可靠性：hadoop各个节点存储的数据，有若干个副本，分布在不同的节点上，即使单个节点数据丢失，会从副本节点恢复数据。
2. 高扩展性：hadoop在集群间分配任务数据，可以方便的动态增加节点，无需停止服务。
3. 高效性：hadoop是分布式的集群，其中基于mapReduce的思想，各节点并行工作，处理任务的效率非常高。
4. 高容错性：当因在处理任务中的某个环节失败时，hadoop会自动重新分配任务，保证任务正常进行。

## hadoop的组成
> hadoop的组成分为2个阶段：1.X => 2.X
> 1.X ：
> - mapReduce:兼顾计算和资源调度
> - HDFS：数据存储
> - common：辅助工具
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

## 