# 区别

不同点：

（1）提供方：@Autowired是由org.springframework.beans.factory.annotation.Autowired提供，换句话说就是由Spring提供；@Resource是由javax.annotation.Resource提供，即J2EE提供，需要JDK1.6及以上。

（2）注入方式：@Autowired只按照byType 注入；@Resource默认按byName自动注入，也提供按照byType 注入；

### @Autowired

1： @Autowired只按照byType 注入

2： @Autowired按类型装配依赖对象，默认情况下它要求依赖对象必须存在 

3： 如果我们想使用按名称装配，可以结合@Qualifier注解一起使用 

### @Autowired(required=false)

如果允许null值，可以设置它required属性为false 

###  @Resource 

　1. 如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常

　　2. 如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常

　　3. 如果指定了type，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常

　　4. 如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配； 

