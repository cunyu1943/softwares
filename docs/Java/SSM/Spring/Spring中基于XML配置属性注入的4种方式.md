[TOC]

## 1. 前言

在以 XML 配置的 Spring 项目中，我们可以通过多种方式来实现属性的注入。而最主要的方式无非以下几种：

-   **构造方法注入**
-   **set 方法注入**
-   **p 名称空间注入**
-   **外部 Bean 的注入**
-   **对象、数组等复杂属性的注入**

接下来就对着几种属性注入的方式进行一一展开描述。

## 2. 构造方法注入

1.  要给 Bean 的属性注入值，那么首先需要给 Bean 添加对应构造方法

```java
package com.cunyu.domain;

/**
 * @author : cunyu
 * @version : 1.0
 * @className : Book
 * @date : 2020/7/7 14:10
 * @description : Book 类
 */

public class Book {
    private Integer id;
    private String name;
    private Double price;

    public Book() {
        System.out.println("Book 类初始化");
    }

    public Book(Integer id, String name, Double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }
}
```

2.  构造方法添加之后，紧接着到 `applicationContext.xml` 文件中注入 Bean；

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean class="com.cunyu.domain.Book" id="book">
        <constructor-arg index="0" value="1"/>
        <constructor-arg index="1" value="Effective Java"/>
        <constructor-arg index="2" value="119.0"/>
    </bean>

</beans>

```

**注意**：`constructor-arg` 中的 `index` 和 Bean 类中的构造方法参数应该一一对应，顺序可以随意，但 `index` 的值需要和 `value` 保持一致。此外，当参数较多时，我们也可以通过直接指定参数名来进行注入：

```xml
<bean class="com.cunyu.domain.Book" id="book">
    <constructor-arg name="id" value="2"/>
    <constructor-arg name="name" value="算法（第 4 版）"/>
    <constructor-arg name="price" value="149.0"/>
</bean>
```

## 3. set 方法注入

除开构造方法外，还可以通过 `set` 方法来注入。利用 `set` 方法来注入时，通常需要在 `applicationContext.xml` 中进行注入，然后获取参数属性值即可。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean class="com.cunyu.domain.Book" id="book">
        <property name="id" value="3"/>
        <property name="name" value="剑指 Offer（第 2 版）"/>
        <property name="price" value="89.0"/>
	</bean>

</beans>

```

## 4. p 名称空间注入

很少会使到的一种方式，其本质也是调用 `set` 方法；

```xml
<bean class="com.cunyu.domain.Book" id="book" p:id="4" p:bookName="Java 并发编程的艺术" p:price="67.0"></bean>
```

## 5. 外部 Bean 的注入

在 OkHttp 的网络请求中，原生的写法如下：

```java
public class OkHttpMain {
    public static void main(String[] args) {
        OkHttpClient okHttpClient = new OkHttpClient.Builder()
                .build();
        Request request = new Request.Builder()
                .get()
                .url("http://cunyu1943.github.io/imgs/1.jpg")
                .build();
        Call call = okHttpClient.newCall(request);
        call.enqueue(new Callback() {
            @Override
            public void onFailure(@NotNull Call call, @NotNull IOException e) {
                System.out.println(e.getMessage());
            }

            @Override
            public void onResponse(@NotNull Call call, @NotNull Response response) throws IOException {
                FileOutputStream out = new FileOutputStream(new File("/home/cunyu/1.jpg"));
                int len;
                byte[] buf = new byte[1024];
                InputStream is = response.body().byteStream();
                while ((len = is.read(buf)) != -1) {
                    out.write(buf, 0, len);
                }
                out.close();
                is.close();
            }
        });
    }
}
```

这个 Bean 有一个特点，OkHttpClient 和 Request 两个实例都不是直接 new 出来的，在调用 Builder 方法的过程中，都会给它配置一些默认的参数。这种情况，我们可以使用 静态工厂注入或者实例工厂注入来给 OkHttpClient 提供一个实例。

### 5.1 静态工厂注入

1.  首先提供一个 OkHttpClient 的静态工厂：

```java
public class OkHttpUtils {
    private static OkHttpClient OkHttpClient;
    public static OkHttpClient getInstance() {
        if (OkHttpClient == null) {
            OkHttpClient = new OkHttpClient.Builder().build();
        }
        return OkHttpClient;
    }
}
```

2.  然后在 xml 文件中，配置该静态工厂：

```xml
<bean class="com.cunyu.OkHttpUtils" factory-method="getInstance" id="okHttpClient"></bean>
```

这个配置表示 OkHttpUtils 类中的 getInstance 是我们需要的实例，实例的名字就叫 okHttpClient。

3.  最后，在我的业务代码中获取到这个实例，就可以直接使用了。

```java
public class OkHttpMain {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        OkHttpClient okHttpClient = ctx.getBean("okHttpClient", OkHttpClient.class);
        Request request = new Request.Builder()
                .get()
                .url("http://cunyu1943.github.io/imgs/1.jpg")
                .build();
        Call call = okHttpClient.newCall(request);
        call.enqueue(new Callback() {
            @Override
            public void onFailure(@NotNull Call call, @NotNull IOException e) {
                System.out.println(e.getMessage());
            }

            @Override
            public void onResponse(@NotNull Call call, @NotNull Response response) throws IOException {
                FileOutputStream out = new FileOutputStream(new File("/home/cunyu/1.jpg"));
                int len;
                byte[] buf = new byte[1024];
                InputStream is = response.body().byteStream();
                while ((len = is.read(buf)) != -1) {
                    out.write(buf, 0, len);
                }
                out.close();
                is.close();
            }
        });
    }
}
```

### 5.2 实例工厂注入

实例工厂就是工厂方法是一个实例方法，这样，工厂类必须实例化之后才可以调用工厂方法。

```java
public class OkHttpUtils {
    private OkHttpClient OkHttpClient;
    public OkHttpClient getInstance() {
        if (OkHttpClient == null) {
            OkHttpClient = new OkHttpClient.Builder().build();
        }
        return OkHttpClient;
    }
}
```

此时，在 xml 文件中，需要首先提供工厂方法的实例，然后才可以调用工厂方法：

```xml
<bean class="com.cunyu.OkHttpUtils" id="okHttpUtils"/>
<bean class="okhttp3.OkHttpClient" factory-bean="okHttpUtils" factory-method="getInstance" id="okHttpClient"></bean>
```

## 6. 对象、数组等复杂属性的注入

### 6.1 对象注入

通过 XML 实现对象注入，需要通过 `ref` 属性来引用一个对象；

```xml
<bean class="com.cunyu.domain.User" id="user">
    <property name="book" ref="book"/>
</bean>
<bean class="com.cunyu.domain.Cat" id="book">
    <property name="id" value="1"/>
    <property name="name" value="数据结构"/>
    <property name="price" value="45.0"/>
</bean>
```

### 6.2 数组注入

数组和列表注入在 XML 中的配置都是一样的；

```java
<bean class="com.cunyu.domain.User" id="user">
    <property name="book" ref="book"/>
    <property name="popular">
    	<array>
    		<value>剑指 Offer（第 2 版）</value>
    		<value>算法（第 4 版）</value>    
	    </array>
    </property>
    <property name="favorite">
    	<list>
		    <value>算法（第 4 版）</value>
	    </list>
    </property>
</bean>
<bean class="com.cunyu.domain.Cat" id="book">
    <property name="id" value="1"/>
    <property name="name" value="数据结构"/>
    <property name="price" value="45.0"/>
</bean>
```

### 6.3 Map 注入

```xml
<property name="map">
    <map>
        <entry key="id" value="99"/>
        <entry key="name" value="深入理解 Java 虚拟机"/>
    </map>
</property>
```

### 6.4 Properties 注入

```xml
<property name="info">
    <props>
        <prop key="id">100</prop>
        <prop key="name">数学之美</prop>
    </props>
</property>
```



![](https://gitee.com/cunyu1943/images/raw/master/ImgsUbuntu/20200510234310.png)

---
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/css/share.min.css">
<center><div class="social-share"></div></center>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js"></script>