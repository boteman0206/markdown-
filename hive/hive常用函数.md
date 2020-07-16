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

##### explode

```sql
列转行常用函数 ： 将array转成 1列n行的的格式  只能单独使用
select explode (array("jack", "name")); 

炸裂函数 配合 lateral view  进行炸裂之后的笛卡尔积 city中的每一行都会和jack， name进行关联
select * from city lateral view  explode (array("jack", "name"))  temp as cname;

```

![image-20200716221929956](../picture\image-20200716221929956.png)

