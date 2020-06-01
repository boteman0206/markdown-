## 原生api： jedis

##### 导入依赖文件

```java
<dependency>
   <groupId>redis.clients</groupId>
   <artifactId>jedis</artifactId>
   <version>3.2.0</version>
</dependency>
```



**基本命令基本和redis客户端差不多**

```java
Jedis jedis = new Jedis("127.0.0.1", 6379);
Boolean money = jedis.exists("money");


// 开启事务
 Transaction multi = jedis.multi();
// ... 程序代码块
 multi.exec();  // 执行事务
 multi.discard(); // 放弃事务  可以使用try .. catch
```



