#### 新增原因

````java
因为旧的java.util.Date实在是太难用了。
java.util.Date月份从0开始，一月是0，十二月是11，变态吧！java.time.LocalDate月份和星期都改成了enum，就不可能再用错了。

// 注意点
// java.util.Date和SimpleDateFormatter都不是线程安全的，而LocalDate和LocalTime和最基本的String一样，是不变类型，不但线程安全，而且不能修改。

java.util.Date是一个“万能接口”，它包含日期、时间，还有毫秒数，如果你只想用java.util.Date存储日期，或者只存储时间，那么，只有你知道哪些部分的数据是有用的，哪些部分的数据是不能用的。在新的Java 8中，日期和时间被明确划分为LocalDate和LocalTime，LocalDate无法包含时间，LocalTime无法包含日期。当然，LocalDateTime才能同时包含日期和时间。
````



#### LocalDate基本使用 

```java
// 取当前日期：
LocalDate today = LocalDate.now(); // -> 2014-12-24
// 根据年月日取日期，12月就是12：
LocalDate crischristmas = LocalDate.of(2014, 12, 25); // -> 2014-12-25
// 根据字符串取：
LocalDate endOfFeb = LocalDate.parse("2014-02-28"); // 严格按照ISO yyyy-MM-dd验证，02写成2都不行，当然也有一个重载方法允许自己定义格式
LocalDate.parse("2014-02-29"); // 无效日期无法通过：DateTimeParseException: Invalid date
```

#### 日期转换

```java
// 取本月第1天：
LocalDate firstDayOfThisMonth = today.with(TemporalAdjusters.firstDayOfMonth()); // 2014-12-01
// 取本月第2天：
LocalDate secondDayOfThisMonth = today.withDayOfMonth(2); // 2014-12-02
// 取本月最后一天，再也不用计算是28，29，30还是31：
LocalDate lastDayOfThisMonth = today.with(TemporalAdjusters.lastDayOfMonth()); // 2014-12-31
// 取下一天：
LocalDate firstDayOf2015 = lastDayOfThisMonth.plusDays(1); // 变成了2015-01-01
// 取2015年1月第一个周一，这个计算用Calendar要死掉很多脑细胞：
LocalDate firstMondayOf2015 = LocalDate.parse("2015-01-01").with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY)); // 2015-01-05
```

### LocalTime

````java
LocalTime包含毫秒：

LocalTime now = LocalTime.now(); // 11:09:09.240
你可能想清除毫秒数：

LocalTime now = LocalTime.now().withNano(0)); // 11:09:09
构造时间也很简单：

LocalTime zero = LocalTime.of(0, 0, 0); // 00:00:00
LocalTime mid = LocalTime.parse("12:00:00"); // 12:00:00
````

### JDBC

````java
SQL -> Java
--------------------------
date -> LocalDate
time -> LocalTime
timestamp -> LocalDateTime
````



