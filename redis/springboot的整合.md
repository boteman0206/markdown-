## SpringData 是数据整合的一个框架

```java
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```



**说明： 在springBoot之后，原来使用的jedis被替换成为了lettuce**

```tex
1： jedis： 采用的是智联，多个线程操作的话是不安全的， 如果想要避免不安全，使用jedis pool连接池。
2： lettuce：采用的是netty，实例可以在多个线程中进行共享，不存在线程不安全的情况，可以吉安扫线程的数据。
```



##### 源码分析

`1: 找到Autoconfigure包`

![image-20200601222548675](..\picture\image-20200601222548675.png)

`2: 找到redis的配置类`

![image-20200601222636928](..\picture\image-20200601222636928.png)

`3： 找到 redis的配置类文件`

![image-20200601222723013](..\picture\image-20200601222723013.png)

`4:  查看文件中可以配置的选项`

![image-20200601222734258](..\picture\image-20200601222734258.png)



#### 主要template对象

![image-20200601223430178](..\picture\image-20200601223430178.png)

#### 使用操作（没有序列化会出现乱码）

![image-20200601224357449](..\picture\image-20200601224357449.png)

##### 序列化配置， 默认方式是jdk序列化

![image-20200601224518276](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200601224518276.png)



#### 自定义template进行序列化的配置

```java
package com.marcpoint.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.*;
import org.springframework.data.redis.serializer.StringRedisSerializer;

/**
 * Redis配置
 */
@Configuration
public class RedisConfig {
    @Autowired
    private RedisConnectionFactory factory;

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new StringRedisSerializer());
        redisTemplate.setConnectionFactory(factory);
        return redisTemplate;
    }

    @Bean
    public HashOperations<String, String, Object> 		             	hashOperations(RedisTemplate<String, Object> redisTemplate) {
        return redisTemplate.opsForHash();
    }

    @Bean
    public ValueOperations<String, String> 					 	 	valueOperations(RedisTemplate<String, String> redisTemplate) {
        return redisTemplate.opsForValue();
    }

    @Bean
    public ListOperations<String, Object> 					listOperations(RedisTemplate<String, Object> redisTemplate) {
        return redisTemplate.opsForList();
    }

    @Bean
    public SetOperations<String, Object> setOperations(RedisTemplate<String, Object> redisTemplate) {
        return redisTemplate.opsForSet();
    }

    @Bean
    public ZSetOperations<String, Object> zSetOperations(RedisTemplate<String, Object> redisTemplate) {
        return redisTemplate.opsForZSet();
    }
}

```

​	