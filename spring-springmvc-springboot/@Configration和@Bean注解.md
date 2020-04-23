### springboot--常用注解--@configration、@Bean



##### @Configration：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {
    String value() default "";
}
```

``1： @Configuration底层是含有@Component ，所以@Configuration 具有和 @Component 的作用。``

``2：@Configuration可理解为用spring的时候xml里面的<beans>标签。``

##### @Bean：

``1：@Bean可理解为用spring的时候xml里面的<bean>标签 ``

``2: @Bean标注在方法上(返回某个实例的方法)，等价于spring的xml配置文件中的<bean>，作用为：注册bean`对象``



### 示例：

```java
package com.dsx.demo;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

@Configuration
public class TestConfiguration {
    public TestConfiguration() {
        System.out.println("TestConfiguration容器启动初始化。。。");
    }

    // @Bean注解注册bean,同时可以指定初始化和销毁方法
    @Bean
    @Scope("prototype")
    public TestBean testBean() {
        return new TestBean();
    }
}
```

**上述操作相当于实例化TestBean ，并交给spring管理。**

注： 
``(1)、@Bean注解在返回实例的方法上，如果未通过@Bean指定bean的名称，则默认与标注的方法名相同；``
``(2)、@Bean注解默认作用域为单例singleton作用域，可通过@Scope(“prototype”)设置为原型作用域； ``
``(3)、既然@Bean的作用是注册bean对象，那么完全可以使用@Component、@Controller、@Service、@Repository等注解注册bean（在需要注册的类上加注解），当然需要配置@ComponentScan注解进行自动扫描。``



##### **如果要让一个普通类交给Spring容器管理，通常有以下方法：** 

``1、使用 @Configuration与@Bean 注解``

``2、使用@Controller @Service @Repository @Component 注解标注该类，然后启用@ComponentScan自动扫描``

``3、使用@Import 方法``

 

 

 

 

 