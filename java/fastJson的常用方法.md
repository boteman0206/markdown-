##### 1： 解析对象成为json字符串

```java
 JSON.toJsonString(Object o);   
 JSON.toJSONString(对象);
```



##### 2:  将字符串解析成JSON对象

```java
// 解析单个对象
// 1： 方式一
JSON.parseObject(String json,Class.clazz); // 解析成javaBean对象
// 2: 方式二 和上面差不多
StudentEntity studentEntity2 = JSON.parseObject(string, new TypeReference<StudentEntity>() {

// 解析数组对象
// 1: 方式一
List<StudentEntity> studentEntities = JSON.parseArray(string, StudentEntity.class);
//  2: 方式二
List<Student> stu =JSON.parseObject(jsonStu, new TypeReference<List<Student>>(){}); 
```



##### 3: 其他常用类库

```java
public static final Object parse(String text); // 把JSON文本parse为JSONObject或者JSONArray 
public static final JSONObject parseObject(String text)； // 把JSON文本parse成JSONObject    
public static final <T> T parseObject(String text, Class<T> clazz); // 把JSON文本parse为JavaBean 
public static final JSONArray parseArray(String text); // 把JSON文本parse成JSONArray 
public static final <T> List<T> parseArray(String text, Class<T> clazz); //把JSON文本parse成JavaBean集合 
public static final String toJSONString(Object object); // 将JavaBean序列化为JSON文本 
public static final String toJSONString(Object object, boolean prettyFormat); // 将JavaBean序列化为带格式的JSON文本 
public static final Object toJSON(Object javaObject); 将JavaBean转换为JSONObject或者JSONArray。
```

