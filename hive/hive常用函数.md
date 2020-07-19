##### 查看函数的详情

```sql
desc  function extended year; // 查看year函数的使用详情
```

#####  nvl

```sql
类似于sql中的ifnull() 函数； 一般使用在聚合avg，sum之类的操作 将null值进行替换
select nvl(null, 0); // 返回 0
```

##### concat

```sql
字符串的拼接 注意： 有null的情况下返回null
select concat("abc", 'nnm');   -- 	abcnnm
select concat("abc", 'nnm', null );  -- null
```

##### concat_ws

```sql
使用拼接负号进行拼接； 可以拼接字符串和数组
select concat_ws('==', 'vvv',"jiiji") --  	vvv==jiiji
select concat_ws('==', 'vvv',"jiiji", array('pop', 'lll')) -- 	vvv==jiiji==pop==lll
```

##### collect_set

```sql
属于聚合函数
将多个集合返回一个set集合 去重 无顺序的
select collect_set(cityname) from city group by citytier;
配合concat_ws使用， 类似于group_concat的效果， 因为hive没有这个函数
select concat_ws('$',  collect_set(cityname) )  from city group by citytier;
```

##### collect_list

```sql
属于聚合函数 配合group by使用
不去重复
select collect_list(cityname) from city group by citytier;
```

##### explode 和 lateral view

```sql
列转行常用函数 ： 将array转成 1列n行的的格式  只能单独使用
select explode (array("jack", "name")); 

炸裂函数 配合 lateral view  进行炸裂之后的笛卡尔积 city中的每一行都会和jack， name进行关联
select * from city lateral view  explode (array("jack", "name"))  temp as cname;

```

![image-20200716221929956](../picture\image-20200716221929956.png)

#####  CASE WHEN

```sql
select 
  dept_id,
  sum(case sex when '男' then 1 else 0 end) male_count,
  sum(case sex when '女' then 1 else 0 end) female_count
from 
  emp_sex
group by
  dept_id;
```

##### 窗口函数

```sql
-- 1：排序函数
RANK() 排序相同时会重复，总数不会变
DENSE_RANK() 排序相同时会重复，总数会减少
ROW_NUMBER() 会根据顺序计算

-- 2：聚合函数
count(*)|sum(*)|avg(*)... over (partition by fields.. order by fields WINDOW子句) as nt

-- 参考: http://lxw1234.com/archives/2015/04/190.htm
-- 3：其他分析函数 （ 注意： 这几个函数不支持WINDOW子句。）
3.1: LAG： LAG(col,n,DEFAULT) 用于统计窗口内往上第n行值
第一个参数为列名，第二个参数为往上第n行（可选，默认为1），第三个参数为默认值（当往上第n行为NULL时候，取默认值，如不指定，则为NULL）
SELECT cookieid,
createtime,
url,
ROW_NUMBER() OVER(PARTITION BY cookieid ORDER BY createtime) AS rn,
LAG(createtime,1,'1970-01-01 00:00:00') OVER(PARTITION BY cookieid ORDER BY createtime) AS last_1_time,
LAG(createtime,2) OVER(PARTITION BY cookieid ORDER BY createtime) AS last_2_time 
FROM lxw1234;
3.2: LEAD： 与LAG相反
LEAD(col,n,DEFAULT) 用于统计窗口内往下第n行值
第一个参数为列名，第二个参数为往下第n行（可选，默认为1），第三个参数为默认值（当往下第n行为NULL时候，取默认值，如不指定，则为NULL）
3.3： FIRST_VALUE 取分组内排序后，截止到当前行，第一个值。就是去每个分组中的第一个值
SELECT cookieid,
createtime,
url,
ROW_NUMBER() OVER(PARTITION BY cookieid ORDER BY createtime) AS rn,
FIRST_VALUE(url) OVER(PARTITION BY cookieid ORDER BY createtime) AS first1 
FROM lxw1234;
3.4： LAST_VALUE 取分组内排序后，截止到当前行，最后一个值
-- 如果不指定ORDER BY，则默认按照记录在文件中的偏移量进行排序，会出现错误的结果
-- 提示：在使用分析函数的过程中，要特别注意ORDER BY子句，用的不恰当，统计出的结果就不是你所期望的。
http://lxw1234.com/archives/2015/04/185.htm
3.5: CUME_DIST: –CUME_DIST 小于等于当前值的行数/分组内总行数
–比如，统计小于等于当前薪水的人数，所占总人数的比例
3.6: PERCENT_RANK –PERCENT_RANK 分组内当前行的RANK值-1/分组内总行数-1
应用场景不了解，可能在一些特殊算法的实现中可以用到吧。
```

##### substring

```sql
字符串截取函數
select substring("2017-09-13", 1, 7); --  起始位置 1  输出：  2017-09
```

