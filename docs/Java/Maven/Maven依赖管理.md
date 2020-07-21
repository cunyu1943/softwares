[TOC]

## 1. 前言

在我们创建使用 Maven 项目的过程中，当需要用到第三方的控件时，都是通过依赖管理来达成，也就是 Maven 项目中必有的 `pom.xml` 文件。POM（Project Object Model），即 **项目对象模型**，其中定义了 Maven 项目的形式。因此，`pom.xml` 可以看做是 Maven 项目中的导航。

## 2. Maven 坐标

一个坐标的组成一般有如下几部分，前三者必须，`packaging` 可选，`classifier` 不能直接定义。

-   **groupId**：定义 Maven 项目隶属的实际组织，一般约定以创建该项目的组织名称的逆向域名开头。
-   **artifactId**：定义实际项目中的一个 Maven 项目（模块），推荐使用实际项目名作为前缀。
-   **version**：定义 Maven 项目当前所处版本。
-   **packaging**：项目打包方式，默认使用 `jar`。
-   **classifier**：帮助定义构建输出的一些附属构建，与主构件对应。
-   **dependencies**：添加项目所需的 `jar` 所对应的 Maven 坐标。
-   **dependency**：`dependencies` 的一个子标签，一个 `dependency` 对应一个坐标。
-   **scope**：表示依赖的范围，通常有如下几种：

| 依赖范围   | 编译期有效 | 测试期有效 | 运行时有效 | 打包有效 |
| ---------- | ---------- | ---------- | ---------- | -------- |
| `compile`  | 😄          | 😄          | 😄          | 😄        |
| `test`     | 😡          | 😄          | 😡          | 😡        |
| `privided` | 😄          | 😄          | 😡          | 😡        |
| `runtime`  | 😡          | 😄          | 😄          | 😄        |
| `system`   | 😄          | 😄          | 😡          | 😡        |

## 3. 依赖冲突

### 3.1 冲突产生原因

Maven 项目中，通常都会定义血多 `dependency`，每个 `dependency` 内部也会定义它的 `dependency`，而有时各个依赖之间会产生冲突，冲突的原因通常主要是 **由于 `jar` 包依赖的传递性**，如果在一个项目中同时引入了一个依赖的不同版本，就可能导致依赖冲突。

### 3.2 解决冲突的办法

当冲突产生时，需要如何解决呢？通常我们有两种处理策略：

-   **Maven 的默认处理策略**：

1.  **最短路径优先**：对于不同路径长度的 `jar` 包，优先选择路径更短的生效。
2.  **最先声明优先**：当路径一样时，如 `A -> B -> C` ，`E -> F -> C`，那么则谁先声明则先选择谁生效。

-   **移除依赖：用于排除某项依赖的依赖包**

除开上述策略外，我们也可以手动在 `pom.xml` 中使用 `<exclusion>` 标签来排除发生冲突的依赖包，如下面用于排除 `sring-core` 冲突的例子：

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.1.9.RELEASE</version>
    <exclusions>
        <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

![](https://gitee.com/cunyu1943/images/raw/master/ImgsUbuntu/20200510234310.png)

---
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/css/share.min.css">
<center><div class="social-share"></div></center>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js"></script>