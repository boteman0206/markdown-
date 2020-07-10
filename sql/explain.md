<https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&mid=2247484461&idx=2&sn=5469534e2f370aba86c3a24a2ff52b70&chksm=ebd7452cdca0cc3ad456d695a78f48e72c245f85b4afb210fb7b62218e89785d964d72ec4891&token=620000779&lang=zh_CN&scene=21###wechat_redirect> 

#### 1： 读懂explain命令结果

```tex
explain命令输出的结果有10列：id、select_type、table、type、possible_keys、key、key_len、ref、rows、Extra
```

##### 1.1:  id

```text
包含一组数字，表示查询中执行SELECT子句或操作表的顺序
	1.1.1: 如果id相同执行顺序由上至下。
	1.1.2: 如果id不相同，id的序号会递增，id值越大优先级越高，越先被执行。
(一般有子查询的SQL语句id就会不同)
```

##### 1.2:  select_type: 表示select查询的类型

```tex
select_type属性下有好几种类型：
1.2.1: SIMPLLE(常见)：简单查询，该查询不包含 UNION 或子查询
1.2.2：PRIMARY(常见)：如果查询包含UNION 或子查询，则最外层的查询被标识为PRIMARY
1.2.3： UNION：表示此查询是 UNION 中的第二个或者随后的查询:
1.2.4：DEPENDENT：UNION 满足 UNION 中的第二个或者随后的查询，其次取决于外面的查询
1.2.5： UNION RESULT：UNION 的结果
1.2.6： SUBQUERY(常见)：子查询中的第一个select语句(该子查询不在from子句中)
1.2.7： DEPENDENT SUBQUERY：子查询中的 第一个 select，同时取决于外面的查询
1.2.8： DERIVED(常见)：包含在from子句中子查询(也称为派生表)
1.2.9： UNCACHEABLE SUBQUERY：满足是子查询中的第一个 select 语句，同时意味着 select 中的某些特性阻止结果被缓存于一个 Item_cache 中
1.2.10： UNCACHEABLE UNION：满足此查询是 UNION 中的第二个或者随后的查询，同时意味着 select 中的某些特性阻止结果被缓存于一个 Item_cache 中
```

#### 1.3:  tables

```tex
该列显示了对应行正在访问哪个表(有别名就显示别名)。
当from子句中有子查询时，table列是 <derivenN>格式，表示当前查询依赖 id=N的查询，于是先执行 id=N 的查询
```

#### 1.4: type（重要依据）

```tex
该列称为关联类型或者访问类型，它指明了MySQL决定如何查找表中符合条件的行，同时是我们判断查询是否高效的重要依据。
以下为常见的取值
1.4.1： ALL：全表扫描，这个类型是性能最差的查询之一。通常来说，我们的查询不应该出现 ALL 类型，因为这样的查询，在数据量最大的情况下，对数据库的性能是巨大的灾难。
1.4.2： index：全索引扫描，和 ALL 类型类似，只不过 ALL 类型是全表扫描，而 index 类型是扫描全部的索引，主要优点是避免了排序，但是开销仍然非常大。如果在 Extra 列看到 Using index，说明正在使用覆盖索引，只扫描索引的数据，它比按索引次序全表扫描的开销要少很多。
1.4.3： range：范围扫描，就是一个有限制的索引扫描，它开始于索引里的某一点，返回匹配这个值域的行。这个类型通常出现在 =、&lt;&gt;、&gt;、&gt;=、<、<=、IS NULL、<=>、BETWEEN、IN() 的操作中，key 列显示使用了哪个索引，当 type 为该值时，则输出的 ref 列为 NULL，并且 key_len 列是此次查询中使用到的索引最长的那个。
1.4.4： ref：一种索引访问，也称索引查找，它返回所有匹配某个单个值的行。此类型通常出现在多表的 join 查询, 针对于非唯一或非主键索引, 或者是使用了最左前缀规则索引的查询。
1.4.5： eq_ref：使用这种索引查找，最多只返回一条符合条件的记录。在使用唯一性索引或主键查找时会出现该值，非常高效。
1.4.6： const、system：该表至多有一个匹配行，在查询开始时读取，或者该表是系统表，只有一行匹配。其中 const 用于在和 primary key 或 unique 索引中有固定值比较的情形。
1.4.7： NULL：在执行阶段不需要访问表。
```

#### 1.5： possible_keys

```tex
这一列显示查询可能使用哪些索引来查找
```

#### 1.6: key

```tex
这一列显示MySQL实际决定使用的索引。如果没有选择索引，键是NULL。
```

#### 1.7: key_len

```tex
这一列显示了在索引里使用的字节数，当key列的值为 NULL 时，则该列也是 NULL
```

### 1.8: ref

```tex
这一列显示了哪些字段或者常量被用来和key配合从表中查询记录出来。
```

#### 1.9: rows

```tex
这一列显示了估计要找到所需的行而要读取的行数，这个值是个估计值，原则上值越小越好。
```

#### 1.10: extra

```tex
其他的信息
Using index：使用覆盖索引，表示查询索引就可查到所需数据，不用扫描表数据文件，往往说明性能不错。

Using Where：在存储引擎检索行后再进行过滤，使用了where从句来限制哪些行将与下一张表匹配或者是返回给用户。

Using temporary：在查询结果排序时会使用一个临时表，一般出现于排序、分组和多表 join 的情况，查询效率不高，建议优化。

Using filesort：对结果使用一个外部索引排序，而不是按索引次序从表里读取行，一般有出现该值，都建议优化去掉，因为这样的查询 CPU 资源消耗大。
```

