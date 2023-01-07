# load
    load data [local] inpath '文件的路径' into table 表名 [partition (partcol1=val1,particol2=val2...)];
## 案例1：从本地导入-copy动作
    load data local inpath '本地路径' [overwrite] into table tableName;
## 案例2：从HDFS导入-移动的动作
    load data inpath 'HDFS的路径' [overwrite] into table tableName;

# insert
## 案例1(将查询的结果插入到表中--注意查询的字段和被插入的表的字段的个数和类型)
    insert (into|overwrite) table tableName [partition (partcol1=val1,partcol2=val2...)] select语句;
## 案例2(直接向表中插入数据)
    insert into table tableName (字段名1,...)[partcol1=val1,...] values(值1,...);
## 案例3(将查询的结果导出到指定路径)
    insert overwrite [local] directory 路径 [row format row_format][stored as file_format] select语句;
### 导出到本地
    insert overwrite local directory '/home/atguigu/datas/' row format delimited fields terminated by '\t' select * from student;
### 导出到HDFS
    insert overwrite directory '/datas/student' row format delimited fields terminated by '\t' select * from student;

