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

1. 使用`Collectors.toMap()`生成的收集器，用户需要指定如何生成*Map*的*key*和*value*。
2. 使用`Collectors.partitioningBy()`生成的收集器，对元素进行二分区操作时用到。
3. 使用`Collectors.groupingBy()`生成的收集器，对元素做*group*操作时用到。

```java
// 情况1：使用toMap()生成的收集器，这种情况是最直接的，前面例子中已提到，这是和Collectors.toCollection()并列的方法。如下代码展示将学生列表转换成由<学生，GPA>组成的Map。非常直观，无需多言。

// 使用toMap()统计学生GPA
Map<Student, Double> studentToGPA =
  students.stream().collect(Collectors.toMap(Functions.identity(),// 如何生成key
student -> computeGPA(student)));// 如何生成value
```

```java
// 情况2：使用partitioningBy()生成的收集器，这种情况适用于将Stream中的元素依据某个二值逻辑（满足条件，或不满足）分成互补相交的两部分，比如男女性别、成绩及格与否等。下列代码展示将学生分成成绩及格或不及格的两部分。

// Partition students into passing and failing
Map<Boolean, List<Student>> passingFailing = students.stream()
         .collect(Collectors.partitioningBy(s -> s.getGrade() >= PASS_THRESHOLD));
```

```java
//情况3：使用groupingBy()生成的收集器，这是比较灵活的一种情况。跟SQL中的group by语句类似，这里的groupingBy()也是按照某个属性对数据进行分组，属性相同的元素会被对应到Map的同一个key上。下列代码展示将员工按照部门进行分组：

// Group employees by department
Map<Department, List<Employee>> byDept = employees.stream()
            .collect(Collectors.groupingBy(Employee::getDepartment));
```

#### 上下游的收集器

```java
// 以上只是分组的最基本用法，有些时候仅仅分组是不够的。在SQL中使用group by是为了协助其他查询，比如1. 先将员工按照部门分组，2. 然后统计每个部门员工的人数。Java类库设计者也考虑到了这种情况，增强版的groupingBy()能够满足这种需求。增强版的groupingBy()允许我们对元素分组之后再执行某种运算，比如求和、计数、平均值、类型转换等。这种先将元素分组的收集器叫做上游收集器，之后执行其他运算的收集器叫做下游收集器(downstream Collector)。

// 使用下游收集器统计每个部门的人数
Map<Department, Integer> totalByDept = employees.stream()       .collect(Collectors.groupingBy(Employee::getDepartment,
 Collectors.counting()));// 下游收集器
```

#### 更高級的使用

```java
//上面代码的逻辑是不是越看越像SQL？高度非结构化。还有更狠的，下游收集器还可以包含更下游的收集器，这绝不是为了炫技而增加的把戏，而是实际场景需要。考虑将员工按照部门分组的场景，如果我们想得到每个员工的名字（字符串），而不是一个个Employee对象，可通过如下方式做到：
// 按照部门对员工分布组，并只保留员工的名字
Map<Department, List<String>> byDept = employees.stream()
             .collect(Collectors.groupingBy(Employee::getDepartment, Collectors.mapping(Employee::getName,// 下游收集器
Collectors.toList())));// 更下游的收集器
```

## 使用collect()做字符串join

```java
// join的參數 1：分隔符号 2：前缀 3：后缀  
Stream<String> stream = Stream.of("I", "love", "you");
//String joined = stream.collect(Collectors.joining());// "Iloveyou"
//String joined = stream.collect(Collectors.joining(","));// "I,love,you"
String joined = stream.collect(Collectors.joining(",", "{", "}"));// "{I,love,you}"
```

