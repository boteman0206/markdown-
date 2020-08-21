##### 参考文章

<https://juejin.im/post/6844903660732809224> 

<https://www.lagou.com/lgeduarticle/96517.html> 

##### java对象模型

```TEX
	Java对象保存在堆内存中。在内存中，一个Java对象包含三部分：对象头、实例数据和对齐填充。其中对象头是一个很关键的部分，因为对象头中包含锁状态标志、线程持有的锁等标志。
```

##### oop-klass model

```tex
	HotSpot虚拟机中，对象在内存中存储的布局可以分为三块区域：对象头、实例数据和对齐填充。在虚拟机内部，一个Java对象对应一个instanceOopDesc的对象。其中对象头包含了两部分内容：_mark和_metadata，而实例数据则保存在oopDesc中定义的各种field中。
	
```

#### _mark

```tex
对象头中有和锁相关的运行时数据，这些运行时数据是synchronized以及其他类型的锁实现的重要基础，而关于锁标记、GC分代等信息均保存在_mark中
```



##### oop-klass-klassKlass关系如图： 

##### ![img](../picture/165458268508ac24) 



##### 内存存储

```tex
	对象的实例（instantOopDesc)保存在堆上，对象的元数据（instantKlass）保存在方法区，对象的引用保存在栈上。
	方法区用于存储虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。 所谓加载的类信息，其实不就是给每一个被加载的类都创建了一个 instantKlass对象么。
```

##### 存储结构如下： 

![](../picture/1654582685c7ceba)



##### 总结

```tex
	每一个Java类，在被JVM加载的时候，JVM会给这个类创建一个instanceKlass，保存在方法区，用来在JVM层表示该Java类。当我们在Java代码中，使用new创建一个对象的时候，JVM会创建一个instanceOopDesc对象，这个对象中包含了两部分信息，对象头以及元数据。对象头中有一些运行时数据，其中就包括和多线程相关的锁的信息。元数据其实维护的是指针，指向的是对象所属的类的instanceKlass。

```

