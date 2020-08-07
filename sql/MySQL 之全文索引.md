<https://zhuanlan.zhihu.com/p/35675553> 

##### 概念

```
通过数值比较、范围过滤等就可以完成绝大多数我们需要的查询，但是，如果希望通过关键字的匹配来进行查询过滤，那么就需要基于相似度的查询，而不是原来的精确数值比较。全文索引就是为这种场景设计的。
```

##### 版本支持

```
1: MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引；
2: MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引;
3: 只有字段的数据类型为 char、varchar、text 及其系列才可以建全文索引。
测试或使用全文索引时，要先看一下自己的 MySQL 版本、存储引擎和数据类型是否支持全文索引。
```

##### 创建

```
1.创建表时创建全文索引
create table fulltext_test (
    id int(11) NOT NULL AUTO_INCREMENT,
    content text NOT NULL,
    tag varchar(255),
    PRIMARY KEY (id),
    FULLTEXT KEY content_tag_fulltext(content,tag)  // 创建联合全文索引列
) ENGINE=MyISAM DEFAULT CHARSET=utf8;

2.在已存在的表上创建全文索引
create fulltext index content_tag_fulltext on fulltext_test(content,tag);
3.通过 SQL 语句 ALTER TABLE 创建全文索引
alter table fulltext_test add fulltext index content_tag_fulltext(content,tag);
```

