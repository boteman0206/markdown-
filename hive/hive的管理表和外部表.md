##### 1： 管理表： MANAGED_TABLE

##### 1.1： 管理表的概念

```sql
hive默认创建的表都是所谓的管理表，有时也被称为内部表。因为这种表，Hive会（或多或少地）控制着数据的生命周期。
当我们删除一个管理表时，Hive也会删除这个表中数据。管理表不适合和其他工具共享数据。
desc formatted motherbaby.post; -- 查看表详情
Table Type:         MANAGED_TABLE   
```



##### 1.2： 创建内部表

```sql
-- 1.2.2： 普通创
create table if not exists student2(
id int, name string
)
row format delimited fields terminated by '\t'
stored as textfile
location '/user/hive/warehouse/student2';
-- 1.2.2： 按照查询结果创建
create table if not exists student3 as select id, name from student;
-- 1.2.3： 根据已经存在的表结构创建表
create table if not exists student4 like student;
```



##### 2： 外部表:  EXTERNAL_TABLE

##### 2.1： 外部表的概念

```sql
因为表是外部表，所以Hive并非认为其完全拥有这份数据。删除该表并不会删除掉这份数据，不过描述表的元数据信息会被删除掉。
Table Type:         	EXTERNAL_TABLE    
```

##### 2.2： 创建外部表

```sql
create external table if not exists default.dept(
deptno int,
dname string,
loc int
)
row format delimited fields terminated by '\t';
```



##### 3： 使用场景

```sql
每天将收集到的网站日志定期流入HDFS文本文件。在外部表（原始日志表）的基础上做大量的统计分析，用到的中间表、结果表使用内部表存储，数据通过SELECT+INSERT进入内部表。
```

##### 4: **管理表与外部表的互相转换**

```sql
-- 修改内部表student2为外部表
alter table student2 set tblproperties('EXTERNAL'='TRUE');
-- 修改外部表student2为内部表
alter table student2 set tblproperties('EXTERNAL'='FALSE');
```

