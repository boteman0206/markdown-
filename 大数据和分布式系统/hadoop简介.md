https://zhuanlan.zhihu.com/p/54994736

##### 1： hadoop是什么？

```tex
Hadoop是一个开源的大数据框架，是一个分布式计算的解决方案。 是一种生态。
Hadoop的两个核心解决了数据存储问题（HDFS分布式文件系统）和分布式计算问题（MapRe-duce）。
```

##### 2: Hadoop的核心架构

```tex
Hadoop的核心，说白了，就是HDFS和MapReduce。HDFS为海量数据提供了存储，而MapReduce为海量数据提供了计算框架。
```

![image-20200708232447635](../picture\image-20200708232447635.png)

##### 3： hadoop的生态系统组成

![image-20200708232729995](../picture\image-20200708232729995.png)

##### 4: hadoop 1.x 和 2.x 的区别

```tex
1.x和2.x 的区别是将资源的调度和管理进行了分离！由统一的资源调度平台YARN来进行资源的调度管理，提升hadoop的通用性！
在1.x的版本中 MapReduce： 负责计算，还需要负责计算资源的申请调度！

在hadoop不久之后， 由于MR的低效性， 出现了许多更为好高效的计算框架列如 Tez， Stom， Spark， Flink (这样通过yarn的资源调度可以更具有通用化和切换性)
```

