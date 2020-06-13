##### json 解析工具

**由于fastjson在一些方面的不足，或者说bug比较多， 故而使用jackson对象**

```java
<dependency>
<groupId>com.fasterxml.jackson.core</groupId>
<artifactId>jackson-databind</artifactId>
<version>2.9.1</version>
</dependency>
```

**定义javaBean对象**

```java
import lombok.Builder;
import lombok.Data;
import java.util.Date;

@Data
@Builder
public class User {
    private Integer id;
    private String name;

    private Integer money;

    private Date date;

}

```



##### 1: 将javaBean对象解析成字符串

- writeValue()
- writeValueAsBytes()
- writeValueAsString()

```java
@Test
public void userJson() throws JsonProcessingException {
    ObjectMapper objectMapper = new ObjectMapper();
    // 构建user对象 使用@Builder lombok下面的注解
    User user = User.builder().id( 1 ).name( "liuhailin" ).money( 1000 ).date( new Date() ).build();
    // 将javaBean对象解析为string对象
    String userJson = objectMapper.writeValueAsString( user );
    System.out.println( userJson );
}


// 格式化打印
String userJson = mapper.writerWithDefaultPrettyPrinter().writeValueAsString( user );
```

##### 2: 用Reader反序列化JSON，转Object对象

`jackson可以用抽象类Reader类BufferedReader，CharArrayReader，FilterReader，InputStreamReader，PipedReader，StringReader，反序列化Json数据。`

```java
@AllArgsConstructor
@NoArgsConstructor

// string对象转成javaBean对象 注意点必须要有默认的构造函数
User user1 = mapper.readValue(userJson, User.class);
System.out.println( user1 );

```

##### 3: JSON转HashMap

```java
HashMap userMap = mapper.readValue( str, HashMap.class );
System.out.println( userMap.get( "id" ) );
System.out.println( userMap.get( "name" ) );
```



##### 4: 从文件/流中读取 JSON

```java
@Test
public void jsonToUserByReader() throws IOException {
  String str = "{\n"
    + "  \"id\" : 1,\n"
    + "  \"name\" : \"liuhailin\",\n"
    + "  \"createDate\" : 1509537316534,\n"
    + "  \"money\" : 100.001\n"
    + "}";

  Reader reader = new StringReader( str );
  User user = mapper.readValue( reader, User.class );
  System.out.println( user );
}
```



##### 5: JsonGenerator用于从Java对象生成JSON。 

```java
@Test
public void testJsonGenerator() throws IOException {

  JsonFactory factory = new JsonFactory(  );

  PrintStream stream = System.out;

  JsonGenerator generator = factory.createGenerator( stream, JsonEncoding.UTF8 );
  generator.writeStartObject();
  generator.writeNumberField( "id",2 );
  generator.writeStringField( "name","liuhailin" );
  generator.writeNumberField( "create",System.currentTimeMillis() );
  generator.writeNumberField( "money",1000.01d );
  generator.writeEndObject();

  stream.flush();
  generator.close();
}
```



##### 6： string转成list对象

```java
@Test
void yets1() throws JsonProcessingException {
    ObjectMapper mapper = new ObjectMapper();
    User user = User.builder().id( 1 ).name( "liuhailin" ).money( 1000 ).date( new Date() ).build();
    User user1 = User.builder().id( 1 ).name( "asda" ).money( 1000 ).date( new Date() ).build();
    ArrayList<User> users = new ArrayList<>();
    users.add(user);
    users.add(user1);
    String userJson = mapper.writeValueAsString( users );

    // 方式一：
    JavaType javaType = mapper.getTypeFactory().constructParametricType(List.class,  User.class);
    List<User> users1 = mapper.readValue(userJson, javaType);
    // 方式二：
    //        import  com.fasterxml.jackson.core.type.TypeReference;
    List<User> personList = mapper.readValue(userJson, new TypeReference<List<User>>() {});
        System.out.println(personList);
}
```

##### 7: 转成map

```java
//第二参数是 map 的 key 的类型，第三参数是 map 的 value 的类型
 MapType JavaType = mapper.getTypeFactory().constructMapType(HashMap.class,String.class,Person.class);
 Map<String, Person> personMap = mapper.readValue(JSONInString,JavaType);
 Map<String, Person> personMap = mapper.readValue(JSONInString, new TypeReference<Map<String, Person>>() {});
```

