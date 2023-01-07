# 创建库格式
create database [if not exists] 库名
[comment 库的描述信息]
[location 'hdfs路径'（默认不写使用配置的路径）]
[with dbproperties ("属性名"="属性值",...)];
# 查看库的信息
desc database 库名;
desc database [extended] 库名;可以查看库的属性内容 
# 查看所有的库
show databases [like '模糊查询*|模糊查询*'];
# 选库
use 库名;
# hive官网
https://hive.apache.org

# 修改库
## 修改库格式
- 修改dbproperties:alter database dbName set dbproperties ('属性名'='属性值')
- 修改location:alter database dbName set location path
-修改owner user:alter database dbName set owner user 用户名
# 删除库
drop database [if exists] dbName [restrict|cascade];

