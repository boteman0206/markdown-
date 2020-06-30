**UNION 操作符用于合并两个或多个 SELECT 语句的结果集。  请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。 **

```sql
// 会将name相同的进行去重展示
1： SELECT name FROM TableA UNION SELECT name FROM TableB

// 不会去重展示
2： (2)SELECT name FROM TableA UNION ALL SELECT name FROM TableB
```



**full join和cross join**

不加条件类似于做笛卡尔积