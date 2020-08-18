##### 1： jvm体系结构

![preview](..\picture\v2-dfc3b65b983a82e84420e1335a507e2a_r.jpg)

![image-20200617232105795](../picture\image-20200617232116033.png)

```tex
1.1: new 出来的对象，是存储在堆中的
2.2: class通过new 生成实例对象， 实例对象通过obj.getClass()方法获取Class， class通过class.getClassLoader()获取类加载器。
2.3：静态变量，常量，类信息（构造方法，接口定义），运行时的常量池存在方法区中，但是实例变量存在堆内存中，和方法区无关。
```

##### 2： 类加载器

```tex
assLoader classLoader = Student.class.getClassLoader();
System.out.println(classLoader);
System.out.println(classLoader.getParent());
System.out.println(classLoader.getParent().getParent());
1： 虚拟机自带的加载器  
2： 启动类加载器 // null 1.不存在 2.java获取不到的加载器 加载 rt.jar
3： 扩展类加载器   // ExtClassLoader  加载 jre/lib/ext.jar
4： 应用程序加载器  // appClassLoader
```

##### 3: 双亲委派机制

```tex
1：类加载器收到加载的请求
2： 将这个请求向上委托给父类加载器去完成， 一直向上委托，直到启动类加载器
3： 启动类加载器检查是否能够加载当前的这个类， 能加载就结束，使用当前的加载器，否则，抛出异常，通知子类进行加载
4： 重复步骤3
```

##### 4： 栈： 数据结构

```tex
4.1： 先进后出
4.2： 栈内存，主管程序的运行，生命周期和线程同步
4.3： 8大基本类型 + 对象引用 + 实例方法
```



##### 5： jvm算法总结

https://juejin.im/post/5d59fd5a6fb9a06b017e4d30



##### JVM 的划分及其职能

<https://houbb.github.io/2020/07/19/basic-03-value-pass-ref-pass> 

