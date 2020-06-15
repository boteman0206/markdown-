##### java的并发编程

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

