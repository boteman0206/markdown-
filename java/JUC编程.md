##### java的并发编程 (java.util.concurrent包 )

**1: 获取cpu的核数**

```java
// 获取cpu的核数 // CPU 密集型，IO密集型 System.out.println(Runtime.getRuntime().availableProcessors());

```

##### 2: 线程的几个状态

```java
1： 对于一般的线程来说，是三个状态  

运行态 --->  阻塞态 ----> 就绪态

// java中有一个state的类定义了线程的5个状态
public enum State {
    NEW,  // 新生 
    RUNNABLE,   // 运行 
    BLOCKED,    // 阻塞 
    WAITING,  // 等待，死死地等 
    TIMED_WAITING,  // 超时等待 
    TERMINATED; // 终止 
}
```

##### 5： wait/sleep 区别 

```java
1: 来自不同的类  wait来自 Object  sleep 来自 Thread
2： 关于锁的释放 wait会释放锁，sleep不会释放锁
3： wait必须在同步代码块中使用， sleep 可以再任何地方睡
```

##### 6：  Lock锁（重点）

```java
1: 传统的使用Synchronized 来同步方法或者同步代码块
   对于普通的方法 Synchronized锁的是对象， 对于静态方法来说锁的是class文件
```

###### 什么是AQS  <https://zhuanlan.zhihu.com/p/27134110> 

<https://tech.meituan.com/2019/12/05/aqs-theory-and-apply.html> 

```tex'
java的内置锁一直都是备受争议的，在JDK 1.6之前，synchronized这个重量级锁其性能一直都是较为低下，虽然在1.6后，进行大量的锁优化策略,但是与Lock相比synchronized还是存在一些缺陷的：虽然synchronized提供了便捷性的隐式获取锁释放锁机制（基于JVM机制），但是它却缺少了获取锁与释放锁的可操作性，可中断、超时获取锁，且它为独占式在高并发场景下性能大打折扣。

AQS：AbstractQueuedSynchronizer，即队列同步器。它是构建锁或者其他同步组件的基础框架（如ReentrantLock、ReentrantReadWriteLock、Semaphore等），JUC并发包的作者（Doug Lea）期望它能够成为实现大部分同步需求的基础。它是JUC并发包中的核心基础组件。


```

###### AQS的主要使用方式 

```tex
AQS的主要使用方式是继承，子类通过继承同步器并实现它的抽象方法来管理同步状态。

AQS使用一个int类型的成员变量state来表示同步状态，当state>0时表示已经获取了锁，当state = 0时表示释放了锁。它提供了三个方法（getState()、setState(int newState)、compareAndSetState(int expect,int update)）来对同步状态state进行操作，当然AQS可以确保对state的操作是安全的。

AQS通过内置的FIFO同步队列来完成资源获取线程的排队工作，如果当前线程获取同步状态失败（锁）时，AQS则会将当前线程以及等待状态等信息构造成一个节点（Node）并将其添加到队列末尾，同时会阻塞当前线程，当同步状态释放时，则会把节点中的线程唤醒，使其再次尝试获取同步状态。
```

###### **AQS主要提供了如下一些方法：** 

```tex
getState()：返回同步状态的当前值
setState(int newState)：设置当前同步状态；
compareAndSetState(int expect, int update)：使用CAS设置当前状态，该方法能够保证状态设置的原子性；
tryAcquire(int arg)：独占式获取同步状态，获取同步状态成功后，其他线程需要等待该线程释放同步状态才能获取同步状态
tryRelease(int arg)：独占式释放同步状态；
ryAcquireShared(int arg)：共享式获取同步状态，返回值大于等于0则表示获取成功，否则获取失败；
```

###### 什么是CAS (比较 == 更新)

```java
	JAVA中volatile关键字能解决共享变量的可见性问题，但是不能解决原子性问题，而CAS即compare and swap，它是JDK提供的非阻塞原子性操作，它通过硬件保证了比较-更新操作的原子性。在JDK提供的Unsafe类提供了一系列的compareAndSwap*方法，保证原子性。 CAS有四个操作数，分别是：对象内存位置，对象中变量的偏移量、变量预期值和新的值。操作含义是：如果对象obj中内存偏移量为stateOffset的变量值为except，则使用新的值替换掉旧值except。这是处理器提供的一个原子性操作。我从AQS里复制了一段CAS的源码，
protected final boolean compareAndSetState(int expect, int update) {
    // See below for intrinsics setup to support this
    return unsafe.compareAndSwapInt(this, stateOffset, expect, update);
}
```

#####  什么是自旋锁

```tex
指当一个线程在获取锁的时候，如果锁已经被其它线程获取，那么该线程将循环等待，然后不断的判断锁是否能够被成功获取，直到获取到锁才会退出循环。

自旋锁优缺点
优点
	自旋锁不会使线程状态发生变换，即线程一直都是处于活跃状态，不会被阻塞，减少了CPU上下文切换的开销；
	非自旋锁在获取不到锁的时候会进入阻塞状态，进入内核态，当前面的线程释放锁后，然后又要从内核态切换成用户态去竞争锁，这种来回切换导致锁的性能很差(1.6之前的synchronized)，所以相对来说，自旋转速度更快。

缺点
	自旋锁如果持有锁很长时间，造成其他线程一直在处于死循环中，造成CPU资源浪费，甚至耗尽CPU资源；
	自旋锁没法保证公平性和可重入性。
```



