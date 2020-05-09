#### stream.Collectors工具使用

<https://blog.csdn.net/sl1992/article/details/98900343?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1> 

#### toMap使用

``将list按照年龄分组``

```java
// 使用map进行分组
Stream<UserTest> userStream = Stream.of(new UserTest(0, "张三", 18), new UserTest(1, "张四", 19), new UserTest(2, "张五", 19), new UserTest(3, "老张", 50));
Map<Integer, List<UserTest>> ageMap = userStream.collect(Collectors.toMap(UserTest::getAge,
        Collections::singletonList, (a, b) -> {
    List<UserTest> resultList = new ArrayList<UserTest>(a);
    resultList.addAll(b);
    return resultList;
}));
System.out.println(ageMap);
for (Map.Entry<Integer, List<UserTest>> integerUserTestEntry : ageMap.entrySet()) {
    System.out.println(integerUserTestEntry.getKey() + "=====" + integerUserTestEntry.getValue());
}
```

#### toMap去除重复

```java
Stream<UserTest> userStream = Stream.of(new UserTest(1, "张三", 18), new UserTest(1, "张四", 19), new UserTest(2, "张五", 19), new UserTest(3, "老张", 50));
Map<Integer, String> map = userStream.collect(Collectors.toMap(UserTest::getId, UserTest::getName, (old, newValue) -> old)); 
// 如果有重复的key则保留old
for (Map.Entry<Integer, String> integerStringEntry : map.entrySet()) {
    System.out.println(integerStringEntry.getKey() + "====" + integerStringEntry.getValue());
}
```

#### collect的操作

`实现方式：`

````java
// 方式一：
<R> R collect(Supplier<R> supplier,
                  BiConsumer<R, ? super T> accumulator,
                  BiConsumer<R, R> combiner);
// 方式二：
 <R, A> R collect(Collector<? super T, A, R> collector);

// 很明显第一种相当于简易实现版本,第二种为高级用法.更多更复杂的操作都封装到Collector接口中,并提供一些静态方法供使用者调用.下面逐一分析.
````

**collect的简易调用形式**

```java
// 由于基本类型都是不可变类型,所以这里用数组当做容器
<R> R collect(Supplier<R> supplier,
                  BiConsumer<R, ? super T> accumulator,
                  BiConsumer<R, R> combiner);
// 调用方式如下,很明显第一个参数supplier为结果存放容器,第二个参数accumulator为结果如何添加到容器的操作,第三个参数combiner则为多个容器的聚合策略.

// 示例一
final Integer[] integers = Lists.newArrayList(1, 2, 3, 4, 5)
        .stream()
        .collect(() -> new Integer[]{0}, (a, x) -> a[0] += x, (a1, a2) -> a1[0] += a2[0]);

for (Integer integer : integers) {
    System.out.println(integer);
}
// 示例二 那么再换一种,有一个Person类,其拥有type与name两个属性,那么使用collect把他收集到Map集合中,其中键为type,值为person的集合.如下代码所示,看明白了相信就掌握了该方法.
   Lists.<Person>newArrayList().stream()
        .collect(() -> new HashMap<Integer,List<Person>>(),
            (h, x) -> {
              List<Person> value = h.getOrDefault(x.getType(), Lists.newArrayList());
              value.add(x);
              h.put(x.getType(), value);
            },
            HashMap::putAll
        );
```

**Collector高级调用**

