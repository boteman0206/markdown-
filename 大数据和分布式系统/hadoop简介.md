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

##### 5： HDFS介绍

```tex
HDFS(框架)： 负责大数据的存储
	核心进程（必须的进程）： 
		Namenode（1个）：负责文件，名称等元数据（属性信息）的存储。（列如： 文件名， 文件大小，文件切分了多少个block快，创建和修改时间等信息！）
			  职责： 1： 接受客户端的请求 2： 接受datanode的请求 3：向datanode分配任务
		Datanode（N个）： 负责文件中具体数据的存储。
		      职责： 1：负责接受namenode分配的任务 2： 负责数据块（block）的管理（读写）
	可选进程：
		SecondaryNamenode（N个）： 负责辅助namenode的工作。
```

##### 6： MapReduce

```TEX
MapReduce（编程规范）： 程序中有Mapper（简单处理）和Reducer(合并)
遵循MapReduce的编程规范，编写的程序，打包后，称为一个Job任务！
Job需要提交到YARN上， 向YARN申请计算资源，运行Job中的Task任务（进程）
Job会先创建一个进程MRAppMaster(mapReduce应用的管理者)， 有MRAppMaster向YARN申请资源，
MRAppMaster负责监控Job中各个Task的运行情况，进行容错管理。
```

##### 7： Yarn

```tex
Yarn: 负责集群中的所有计算资源的管理调度。
常见进程： 
	ResourceManager（1个）： 负责整个集群资源的管理 
		职责： 1： 负责接受客户端提交的Job请求！
			  2： 负责向NodeManager分配任务！
			  3： 负责接受NodeManger上报的信息！
	NodeManger（N个）： 负责单台计算机的所有资源的调度和管理
		职责： 1： 负责和ResourceManager进行通信，上报本机中的所有可用的资源！
			  2： 负责和ResourcceManager分配的任务
			  3： 负责为Job中的每一个Task分配计算资源
	概念：
		Container(容器)： NodeManager为Job的某一个Task分配了2个cpu和2g的计算资源，为防止当前Task在使用这些资源的期间，被其他的Task任务抢占，回将计算资源，封装到一个Container中，在Container中的资源，会被暂时的隔离，无法被其他的资源抢占，待当前的task执行结束后，当前的container中的资源会被释放，允许其他的task来使用。
```

