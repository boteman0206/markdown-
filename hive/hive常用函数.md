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

