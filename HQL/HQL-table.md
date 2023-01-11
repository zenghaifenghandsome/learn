# 创建表格式
    create [temporary] [external] table [if not exists] 库名.表名
    [(
        字段名 字段类型 [comment 字段描述],...
    )]
    [comment 描述]
    [partitioned by (字段名 字段类型 [comment 描述],...)]
    [clustered by (字段名,...) [sorted by (字段名[asc|desc],...)]into 桶的数量 buckets]
    [row format 对数据的格式进行说明]
    [stored as 文件的储存格式]
    [location hdfs的路径--默认在库的下面]
    [tblproperties ('属性名'='属性值',...)]
## 案例1
```sql 
create table teacher_json(
name string,
friends array<string>,
students map<string,int>,
address struct<street:string,city:string,postal_code:string>
)
row format serde 'org.apache.hadoop.hive.serde2.JsonSerDe';
```
## 案例2
```sql
create table teacher(
name string,
friends array<string>,
students map<string,int>,
address struct<street:string,city:string,postal_code:string>
)
row format delimited
fields terminated by ','
collection items terminated by '-'
map keys terminated by ':';
```
## 查询数据
    select 字段名,数组类型[索引值],map类型['key'],struct类型.字段名 from 表名;
# 建表
## 案例1
```sql
create table if not exists student(
id int comment 'this is id',
name string comment 'this is name'
)
comment 'this is student'
location '/dbdemo/stu'
tblproperties('name'='stu');
```
## 案例2:外部表和管理表
    external:创建外部表
```sql
create external table external_tbl(
id int,
name string
)
row format delimited fields terminated by '\t';
```
### 管理表-默认
```sql
create table managed_tbl(
id int,
name string
)
row format delimited fields terminated by '\t';
```
### 临时表--当客户端断开连接就会被删除
    temporary:创建临时表
```sql
create temporary table stu(
id int,
name string
);
```
## 基于现有的表创建新表
    create table 表名 like 现有的表名
## 将查询的结果创建成一张新表
    create table 表名 as select语句;


# 表的操作
## 查看所有的表
    show tables [in 库名] [like 模糊查询];
    [in 库名]:查询那个库中的表

## 查看表结构
    desc formatted 表名
    [formatted]:查看表的详细信息
## 删除表
    drop table 表名;

### 在删除表时如果是外部表数据不会被删除，删除管理表时会把数据一起删除
## 修改表的名字
    alter table 原表名 rename to 新表名;
## 添加列
    alter table 表名 add columns (字段名 字段类型 [comment 描述],...);
## 修改列的名字
    alter table 表名 change column 原字段名 新字段名 字段的类型 [comment 描述]
## 修改列的类型--注意类型
    alter table 表名 change column 字段名 字段名 字段的新类型 [comment 描述] [first|after 字段名];
## 管理表和外部表相互转换
    alter table 表名 set tblproperties('EXTERNAL'='TRUE|FALSE');
## 替换列（替换的顺序是依次替换，注意替换的字段的类型，如果替换的字段数少于原字段的个数，那么只显示有字段的列）
    alter table 表名 replace columns (字段名 字段类型 [comment 描述],...)

## 清空表
    清空管理表：直接把表中的内容删除，hdfs中的文件删除
    清空外部表：不能清空会报错
    truncate [table] 表名;



