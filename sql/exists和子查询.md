##### 示例

```sql
-- 使用含有关键字exists查找未分配具体部门的员工的所有信息
select * from employees b where not exists (
    select * from dept_emp a where a.emp_no=b.emp_no
)
```

子查询： https://zhuanlan.zhihu.com/p/20005249  

##### 不相关子查询

```sql
-- 这种类型的查询是先执行子查询，得到一个集合（或值），然后将这个集合（或值）作为一个常量带入到父查询的 WHERE 子句中去。如果单纯地执行子查询，也是可以成功的。
select * from course
where cname in  (select Cname from course where Cname='数学')
```

##### 关联子查询

```sql
-- 大多数情况下，不相关子查询已经够用了，但是如果有这样的一个查询要求：
-- 查询每个学生超过他选修的所有课程的平均成绩的学生信息
子查询可以这么写：
select avg(score) from course where sno=?
```

`具体查询`

![1593506568576](../picture\1593506568576.png)

```tes
其工作原理就是，扫描父查询中数据来源（如 SC 表）中的每一条记录，然后将当前这条记录中的，在子查询中会用到的值代入到子查询中去，然后执行子查询并得到结果（可以看成是返回值），然后再将这个结果代入到父查询的条件中，判断父查询的条件表达式的值是否为 True，若为 True，则将当前 SC 表中的这条记录（经过 SELECT 处理）后放到结果集中去。若为 False 则不放。
```

##### EXISTS 关键字的作用 

```sql
-- 它的作用，就是判断子查询得到的结果集是否是一个空集，如果不是，则返回 True，如果是，则返回 False。

-- 查询所有有部门编号的员工
select * from employees b where exists (
    select * from dept_emp a where a.emp_no=b.emp_no
)
在这个查询中，首先会取出 employees 表中的第一条记录，得到其 emp_no 列（因为在子查询中用到了）的值（如 95001），然后将该值代入到子查询中。若能找到这样的一条记录，那么说明子查询的结果不为空集，那么 EXISTS 会返回 True，从而使 employees 表中的第一条记录中的 employees 的值被放入结果集中去。以此类推，遍历 employees 表中的所有记录后，就能得到所有有部门编号的员工。
```

##### 厉害的一个例子

![1593508774542](../picture\1593508774542.png)

**关联子查询的一个坑**

```sql
-- 示例1： 关联子查询的方式 实现 
# 其中sc是一个多对多的课程和学生的关联表
# 没有学习过任何一门课程的学生信息  注意sc多对多表的放置的顺序
select *
from student t1 where not exists(
      select * from sc t2
            where  exists (
                  select * from course t3 where t3.CId=t2.CId   and  t1.SId=t2.SId)
                       );

-- 示例2： 
# 和上面的查询结果一样，下面例子中将第二层改为 not exists 会得到学全了所有课程的学生， 但是上面的修改将查询不到任何内容
-- 原因： 我们可以理解为关联子查询的嵌套为一个for循环的结构， 当使用上面的多对多的外表做第二层的查询条件时， 修改查询为 not exists 其实是将sc中的所有行进行排除，所以第二层得到的条件永远为false, 接着not exists之后永远为true， 因此第一层的得到的not exists永远为假。
-- 而下面的第二层以course为条件，多以改成not exists之后能得到学全了所有可能的学生。入第三个例子所示。

select *
from student t1 where not exists(
      select * from course t2
            where  exists (
                  select * from sc t3 where t3.CId=t2.CId
                                        and  t1.SId=t3.SId)
                       );


-- 示例3： 学全了所有课程的学生 和上面的查询不一样
select *
from student t1 where not exists(
      select * from course t2
            where not exists (
                  select * from sc t3 where t3.CId=t2.CId
                                        and  t1.SId=t3.SId)
                       );

```

