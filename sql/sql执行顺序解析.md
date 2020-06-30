https://zhuanlan.zhihu.com/p/41844742

##### 1： 普通的单表查询

```tex
1.1: 书写顺序
SELECT》FROM 》WHERE》GROUP BY》HAVE》ORDER BY
1.2: 执行顺序：
FROM 》WHERE》GROUP BY》HAVE》SELECT》ORDER BY
```

#####  2：关联子查询  和 不相关子查询是不一样的

```sql
关联子查询和正常的SELECT语句完全不同。
2.1: 先执行主查询
2.2: 从主查询的product _type先取第一个值=‘衣服’，通过WHERE P1.product_type = P2.product_type传入子查询，子查询变成：
SELECT AVG(sale_price) FROM Product AS P2
WHERE P2.product_type = ‘衣服’
GROUP BY product_type);
从子查询得到的结果AVG(sale_price)=2500，返回主查询：
2.3: 然后，product _type取第二个值，得到整个语句的第二结果，依次类推，把product _type全取值一遍，就得到了整个语句的结果集。结果如下：
```

##### 3: join的执行顺序

https://segmentfault.com/a/1190000015572505

```sql
SELECT <row_list> 
  FROM <left_table> 
    <inner|left|right> JOIN <right_table> 
      ON <join condition> 
        WHERE <where_condition>
它的执行顺序如下(SQL语句里第一个被执行的总是FROM子句)：

3.1: FROM:对左右两张表执行笛卡尔积，产生第一张表vt1。行数为n*m（n为左表的行数，m为右表的行数
3.2: ON:根据ON的条件逐行筛选vt1，将结果插入vt2中
3.3: JOIN:添加外部行，如果指定了LEFT JOIN(LEFT OUTER JOIN)，则先遍历一遍左表的每一行，其中不在vt2的行会被插入到vt2，该行的剩余字段将被填充为NULL，形成vt3；如果指定了RIGHT JOIN也是同理。但如果指定的是INNER JOIN，则不会添加外部行，上述插入过程被忽略，vt2=vt3（所以INNER JOIN的过滤条件放在ON或WHERE里 执行结果是没有区别的，下文会细说）
3.4:  WHERE:对vt3进行条件过滤，满足条件的行被输出到vt4
3.5:  SELECT:取出vt4的指定字段到vt5

```

