<https://juejin.im/post/6844903554264596487> 

```tex
	在并发的情况，发生扩容时，可能会产生循环链表，在执行get的时候，会触发死循环，引起CPU的100%问题，所以一定要避免在并发环境下使用HashMap。
```

 <https://juejin.im/post/6844903682664824845> 

```tex
hashmap头插改成尾插
```

##### 解决方案

```tex
Java中有HashTable、Collections.synchronizedMap、以及ConcurrentHashMap可以实现线程安全的Map。

1: HashTable是直接在操作方法上加synchronized关键字，锁住整个数组，粒度比较大，
2: Collections.synchronizedMap是使用Collections集合工具的内部类，通过传入Map封装出一个SynchronizedMap对象，内部定义了一个对象锁，方法内通过对象锁实现；
3: ConcurrentHashMap使用分段锁，降低了锁粒度，让并发度大大提高。
```

 

##### 使用ConcurrentHashMap 解决

```java
	ConcurrentHashMap是由Segment数组结构和HashEntry数组结构组成。Segment是一种可重入锁ReentrantLock，在ConcurrentHashMap里扮演锁的角色，HashEntry则用于存储键值对数据。一个ConcurrentHashMap里包含一个Segment数组，Segment的结构和HashMap类似，是一种数组和链表结构， 一个Segment里包含一个HashEntry数组，每个HashEntry是一个链表结构的元素， 每个Segment守护者一个HashEntry数组里的元素,当对HashEntry数组的数据进行修改时，必须首先获得它对应的Segment锁。
```

![](../picture/ConcurrentHashMap.jpg)

##### ConcurrentHashMap分段所原理<https://zhuanlan.zhihu.com/p/125628540>  

```tex
ConcurrentHashMap成员变量使用volatile 修饰，免除了指令重排序，同时保证内存可见性，另外使用CAS操作和synchronized结合实现赋值操作，多线程操作只会锁住当前操作索引的节点。
```

![1597644443479](../picture\1597644443479.png)

