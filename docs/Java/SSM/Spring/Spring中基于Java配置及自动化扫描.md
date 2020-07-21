[TOC]

## 1. Java 配置

基于 Java 配置的方式将 Bean 注册到 Spring 容器中，在 Spring Boot 出现前很少使用，但自从 Spring Boot 开始流行，基于 Java 配置的方式就被广泛使用。

### 1.1 基于 Java 配置的实例

```java
public class HelloWorld{
    public String sayHello(){
        return "Hello world!";
    }
}
```

假如我们想把上面的 Bean 注册到 Spring 容器中，我们直接使用一个 Java 配置类就能够代替之前基于 XML 配置的 `applicationContext.xml` 文件；

```java
@Configuration
public class HelloWorldConfig{
    @Bean
    HelloWorld sayHello(){
        return new HelloWorld();
    }
}
```

其中 `@Configuration` 注解表示该类是一个配置类，相当于基于 XML 配置方式中的 `applicationContext.xml`. 然后在方法上添加 `@Bean` 注解，表示将该方法方法的返回值注入 Spring 容器，也即 `@Bean` 所注解的方法，对应 `applicationContext.xml` 中的 `bean` 节点。

最后在项目启动时加载配置类即可。

```java
public class Main{
    public void main(String[] args){
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(HelloWorldConfig.class);
        HelloWorld hello = applicationContext.getBean(HelloWorld.class);
        System.out.println(hello.sayHello());
    }
}
```

## 2. 自动化配置

### 2.1 常用注解

-   **@Service**：用于 `Service` 层，在自动化扫描时，该类将被自动注册到 Spring 容器中；
-   **@Repository**：用于 `Dao` 层，在自动化扫描时，该类将被自动注册到 Spring 容器中；
-   **@Controller**：用于 `Controller` 层，在自动化扫描时，该类将被自动注册到 Spring 容器中；
-   **@Component**：用于其他组件，在自动化扫描时，该类将被自动注册到 Spring 容器中；

### 2.2 自动扫码的两种方式

在 Bean 中添加对应注解之后，Spring 进行自动化扫描工作，主要有以下两种方式：

1.  通过 Java 代码配置；
2.  通过 XML 文件来配置；

### 2.3 Java 代码配置自动扫描

```java
@Configuration
@ComponentScan(basePackages = "com.cunyu.service")
public class UserConfig{
    ...
}
```

然后就可以在项目启动中加载配置类，其中 `@ComponentScan` 注解用于指定要扫描的包，接着就可以获取配置中的实例了；

```java
public class Main{
    public static void main(String[] args) {
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(UserConfig.class);
        UserService userService = applicationContext.getBean(UserService.class);
        System.out.println(userService);
    }
}
```

### 2.4 XML 配置自动化扫描

1.  首先在 `applicationContext.xml` 中配置要扫描的包，然后 Spring 就会自动扫描该包下所有的 Bean，当然指定某一个特定的类来自动扫描也可以：

```xml
<context:component-scan base-package="com.cunyu.service"/>
```

2.  然后在 Java 代码中加载 XML 配置即可；

```java
public class Main{
    public static void main(String[] args){
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = applicationContext.getBean(UserService.class);
        System.out.println(userService);
    }
}
```

## 3. 自动化扫描时的对象注入

自动化扫描时通常有如下三种对象注入的方式：

-   **@Autowired**：根据类型去查找后赋值，类型只能有一个对象，否则将报错。
-   **@Resources**：根据名称去查找，默认情况下定义的变量名即查找的名称，开发者也可以手动指定，适用于存在多个实例的类；
-   **@Injected**：

![](https://gitee.com/cunyu1943/images/raw/master/ImgsUbuntu/20200510234310.png)

---
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/css/share.min.css">
<center><div class="social-share"></div></center>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js"></script>