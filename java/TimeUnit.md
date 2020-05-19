>#### timeUnit的介绍：

````
和Thread.sleep(arg)一样，Thread.sleep(arg)的参数需要传入毫秒级的整数值，如：Thread.sleep(1000)实现了程序延迟1秒钟，如果程序中这样写Thread.sleep(4250000)我们就难以直接的看出程序延迟了多久，而这种情况下，TimeUnit的优势就很明显了,TimeUnit能直观的看出程序延迟多久，代码可读性强。

````

#### 1： 延迟程序

**使用代码对比区别：**

````java
public static void main(String[] args) {
    try {
        Thread.sleep(4250000); //什么玩意
        TimeUnit.SECONDS.sleep(5); //5s
        TimeUnit.MILLISECONDS.sleep(500); //500毫秒
        TimeUnit.DAYS.sleep(1); //一天
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
````

#### 2：时间粒度转换

````java
public static void main(String[] args) {
    TimeUnit timeUnit = TimeUnit.HOURS;
    Long days =timeUnit.convert(1L, TimeUnit.DAYS); //一天有多少小时
    timeUnit = TimeUnit.MILLISECONDS;
    Long milliSeconds = timeUnit.convert(1L, TimeUnit.DAYS); //一天有多少毫秒
    timeUnit = TimeUnit.MINUTES;
    Long minites =  timeUnit.convert(7L, TimeUnit.DAYS); //一周有多少分钟
    
    System.out.println(days);
    System.out.println(milliSeconds);
    System.out.println(minites);
}
:
````



### 常用时间粒度

````java
TimeUnit.DAYS 日的工具类 
TimeUnit.HOURS 时的工具类 
TimeUnit.MINUTES 分的工具类 
TimeUnit.SECONDS 秒的工具类 
TimeUnit.MILLISECONDS 毫秒的工具类

TimeUnit.SECONDS.sleep( 5 );    //延时5秒

TimeUnit.SECONDS.toMillis(1)     // 1秒转换为毫秒数 
TimeUnit.SECONDS.toMinutes(60)   // 60秒转换为分钟数 
TimeUnit.SECONDS.sleep(5)        // 线程休眠5秒 
TimeUnit.SECONDS.convert(1, TimeUnit.MINUTES)   // 1分钟转换为秒数 
````



#### 缺点

``对于一些不满足的时间转换比较棘手列如 ： TimeUnit.HOURS.toDays(23) 将23小时转换为天数的时候为0``