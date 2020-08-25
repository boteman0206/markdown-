参考： <https://juejin.im/post/6844903576339218440> 

​            <https://juejin.im/post/6844903902798675981> 

### `CopyOnWriteArrayList`和synchronizedList的区别和使用

##### `一：CopyOnWriteArrayList`介绍

```java
	从JDK1.5开始Java并发包里提供了两个使用CopyOnWrite机制实现的并发容器,它们是CopyOnWriteArrayList和CopyOnWriteArraySet。CopyOnWrite容器非常有用，可以在非常多的并发场景中使用到。
	CopyOnWriteArrayList原理：	上面已经讲了，就是在写的时候不对原集合进行修改，而是重新复制一份，修改完之后，再移动指针
```

##### `CopyOnWriteArrayList`是怎么解决线程安全问题的 

```java
答案就是----写时复制，加锁
	那么有没有这么一种情况，当一个线程刚好调用完add()方法，也就是刚好执行到上面1处的代码，也就是刚好将引用指向心数组，而此时有线程正在遍历呢？会不会报错呢？（答案是不会的，因为你正在遍历的集合是旧的，这就有点难受啦，哈哈~)
```

##### `CopyOnWriteArrayList`优缺点

```java
缺点：
1、耗内存（集合复制）
2、实时性不高
优点：
1、数据一致性完整，为什么？因为加锁了，并发数据不会乱
2、解决了像ArrayList、Vector这种集合多线程遍历迭代问题，记住，Vector虽然线程安全，只不过是加了synchronized关键字，迭代问题完全没有解决！
```

##### `CopyOnWriteArrayList`使用场景

```tex
1、读多写少（白名单，黑名单，商品类目的访问和更新场景），为什么？因为写的时候会复制新集合
2、集合不大，为什么？因为写的时候会复制新集合
实时性要求不高，为什么，因为有可能会读取到旧的集合数据
```



##### 二： synchronizedList的使用

```java
public void test(){
        ArrayList<String> list = new ArrayList<>();
        List<String> sycList = Collections.synchronizedList(list);
        sycList.add("a");
        sycList.remove("a");
}
从上面的使用方式中我们可以看出，synchronizedList是将List集合作为参数来创建的synchronizedList集合。
其实synchronizedList线程安全的原因是因为它几乎在每个方法中都使用了synchronized同步锁。
```



##### synchronizedList遍历为什么还需要加锁？

```java
synchronizedList官方文档中给出的使用方式是以下方式：
List list = Collections.synchronizedList(new ArrayList());
      ...
  synchronized (list) {
      Iterator i = list.iterator(); // Must be in synchronized block
      while (i.hasNext())
          foo(i.next());
  }
	从以上源码可以看出，虽然内部方法中大部分都已经加了锁，但是iterator方法却没有加锁处理。那么如果我们在遍历的时候不加锁会导致什么问题呢？
试想我们在遍历的时候，不加锁的情况下，如果此时有其他线程对此集合进行add或者remove操作，那么这个时候就会导致数据丢失或者是脏数据的问题，所以如果我们对数据的要求较高，想要避免这方面问题的话，在遍历的时候也需要加锁进行处理。

```



##### 三： synchronizedList和CopyOnWriteArrayList的区别

```text
synchronizedList适合对数据要求较高的情况，但是因为读写全都加锁，所有效率较低。 CopyOnWriteArrayList效率较高，适合读多写少的场景，因为在读的时候读的是旧集合，所以它的实时性不高。 
```

