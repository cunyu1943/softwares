[TOC]

## 1. IDEA 中的 Maven 配置

依次进入 `File -> Settings -> Build,Execution,Deployment -> Build Tools -> Maven`，IDEA 默认使用它自带的 Maven，我们可以自定义为自己的 Maven，更加方便管理，比如我的配置如下：

![](https://s1.ax1x.com/2020/07/07/UFUiPs.png)

## 2. 使用 IDEA 创建 Maven 项目

使用 IDEA 创建 Maven 项目，主要有如下步骤：

1.  `File -> New -> Project`，然后选择 Maven

![](https://s1.ax1x.com/2020/07/07/UFZmMq.png)

2.  填写相关信息

![](https://s1.ax1x.com/2020/07/07/UFZZzn.png)

3.  新建项目完成，完成后的项目目录结构如下

![](https://s1.ax1x.com/2020/07/07/UFZVRs.png)

4.  默认生成的 `pom.xml` 如下

```xml
<!-- 指定 xml 的版本和编码 -->
<?xml version="1.0" encoding="UTF-8"?>
<!-- 所有 pom.xml 的根元素，同时声明一些 pom 相关的命名空间及 xsd 元素-->
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!--  指定 POM 模型版本  -->
    <modelVersion>4.0.0</modelVersion>

    <!--  定义一个项目的基本坐标  -->
    <groupId>com.cunyu</groupId>
    <artifactId>maven-demo</artifactId>
    <version>1.0-SNAPSHOT</version>

</project>

```

5.  到上一步之后，一个新的 Maven 项目就完成了，接下来就是去编写业务代码了。

## 3. 业务代码编写

上面已经学会了如何创建一个 Maven 项目，接下来就是编写业务代码了，我们一经典的 `HelloWorld` 为例：

### 3.1 项目主代码

项目主代码会打包到最终构件中，默认位于 `src/main/java` 目录下，我们创建一个 `HelloWorld` 的主代码；

```java
package com.cunyu.helloworld;

/**
 * @author : cunyu
 * @version : 1.0
 * @className : HelloWorld
 * @date : 2020/6/30 11:06
 * @description : HelloWorld 实例
 */

public class HelloWorld {
    public String sayHello() {
        return "Hello World";
    }

    public static void main(String[] args) {
        System.out.println(new HelloWorld().sayHello());
    }
}
```

### 3.2 项目测试代码

要对主代码进行测试，那么则需要编写测试代码，测试代码默认位于 `src/test/java` 目录，要对指定主代码进行测试，编写测试代码时要和主代码保持相同的目录结构。如上述主代码位于 `com.cunyu.helloworld` 包下，那么测试代码也应该位于 `com.cunyu.helloworld` 包下，只是根目录不同。而要进行测试，通常首选 JUnit 单元测试。所以编写测试代码对主代码进行测试主要有如下步骤。

1.  首先在 `pom.xml` 添加 JUnit 依赖

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>RELEASE</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

2.  接着编写测试代码

```java
package com.cunyu.helloworld;

import org.junit.Test;

import static org.junit.Assert.assertEquals;

/**
 * @author : cunyu
 * @version : 1.0
 * @className : HelloWorldTest
 * @date : 2020/6/30 13:32
 * @description : HelloWorld 测试
 */

public class HelloWorldTest {
    @Test
    public void testSayHello() {
        HelloWorld helloWorld = new HelloWorld();
        String result = helloWorld.sayHello();

        // Assert.assertEquals() 及其重载方法功能:
        // 1. 如果两者一致, 程序继续往下运行.
        // 2. 如果两者不一致, 则中断测试方法,同时抛出异常信息 AssertionFailedError.
        assertEquals("Hello World", result);
        System.out.println("测试通过");
    }
}
```

## 4. 总结

经过上边的项目创建以及业务代码编写之后，一个 Maven 版的 `Hello World` 到此就结束了。接下来就是利用 Maven 的常用命令对主代码和测试代码进行编译测试，然后打包运行即可。是不是很简单呢，赶快自己动手试试吧！

![](https://gitee.com/cunyu1943/images/raw/master/ImgsUbuntu/20200510234310.png)

---
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/css/share.min.css">
<center><div class="social-share"></div></center>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js"></script>