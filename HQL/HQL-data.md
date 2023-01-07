# load
    load data [local] inpath '文件的路径' into table 表名 [partition (partcol1=val1,particol2=val2...)];
## 案例1：从本地导入-copy动作
    load data local inpath '本地路径' [overwrite] into table tableName;
## 案例2：从HDFS导入-移动的动作
    load data inpath 'HDFS的路径' [overwrite] into table tableName;

# insert
