[TOC]

## 1. 什么是 Maven

Maven 是一个项目管理工具，包含了一个项目对象模型（`Project Object Model`），反映在配置中就是 `pom.xml` 文件。其中包含了一个项目的生命周期、一个依赖管理系统，以及定义在项目生命周期阶段的插件（`plugin`）和目标（`goal`）。

其中 Maven 最核心的两大概念包括 **依赖管理** 和 **项目构建**。

-   **依赖管理**：提供对 `jar` 的统一管理。（Maven 提供了一个中央仓库，当我们在项目中添加完依赖后，Maven 就会自动去中央仓库中下载相关依赖）。
-   **项目构建**：Maven 提供对项目的编译、测试、打包、部署、上传到私服等。

## 2. Maven 安装

Maven 属于 Java 项目，因此使用 Maven 必须依赖于 JDK。

![](https://s1.ax1x.com/2020/07/06/UiLG2d.png)

安装好 JDK 之后，然后接下来在安装 Maven，安装过程如下：

1.  下载 Maven，下载地址：https://maven.apache.org/download.cgi

![](https://s1.ax1x.com/2020/07/06/UiLfaT.png)

2.  将下载后的压缩包进行解压

![](https://s1.ax1x.com/2020/07/06/UiOal9.png)

3.  配置环境变量

    -   MAVEN_HOME：即刚才解压缩后 Maven 的存放路径

    ![](https://s1.ax1x.com/2020/07/06/UiXkc9.png)

    -   Path：`%MAVEN_HOME%\bin`

    ![](https://s1.ax1x.com/2020/07/06/UiX4gJ.png)

4.  校验安装是否成功，安装成功则会出现如下提示，校验命令如下：

```bash
mvn -v
```

![](https://s1.ax1x.com/2020/07/06/UijG24.png)

## 3. Maven 目录结构

安装好 Maven 之后，其目录和内容如下，各目录内容如下：

![](https://user-gold-cdn.xitu.io/2020/6/30/17302f7bf71ecf5e?w=1317&h=387&f=png&s=37925)

-   **bin**

包含 mvn 运行的脚步，用于配置 Java 命令，准备好 classpath 和相关的 Java 系统属性，然后执行 Java 命令。

-   **boot**

只包含一个文件，是一个类加载器框架，相对于默认的 Java 类加载器，提供了更吩咐的语法以方便配置。

-   **conf**

包含 `settings.xml` ，通过修改该文件，能在机器中全局定制 Maven 的行为。

-   **lib**

包含所有 Maven 运行时所需的 Java 类库，Maven 本身是分模块开发，所以里边有不同模块之类的类库。此外还包含了一些 Maven 用到的第三方依赖。

## 4. Maven 配置

### 4.1  仓库镜像配置

通常安装好 Maven 之后就可以使用了，但是由于 Maven 的中央仓库服务器位于国外，国内使用网速较慢，所以我们最好将中央仓库换为国内的阿里云镜像。

打开 `apache-maven-xxx/conf/` 目录下的 `settings.xml` 文件，然后在 `mirrors` 节点下加入如下配置：

```xml
<mirror>
    <id>aliyunmaven</id>
    <mirrorOf>*</mirrorOf>
    <name>阿里云公共仓库</name>
    <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```

![](https://s1.ax1x.com/2020/07/06/UixTDs.png)

### 4.2  本地仓库配置

安装好 Maven 后，本地仓库默认在 `当前用户名/.m2/repository` 下，但是这个位置比较隐蔽，所以建议自定义为其他路径：

还是打开 `apache-maven-xxx/conf/` 目录下的 `settings.xml` 文件，然后将如下路径修改为自己要设置的本地仓库，比如我的本地仓库路径如下图：

```xml
<localRepository>/path/to/local/repo</localRepository>
```

![](https://s1.ax1x.com/2020/07/06/UizqLd.png)

![](https://gitee.com/cunyu1943/images/raw/master/ImgsUbuntu/20200510234310.png)

---
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/css/share.min.css">
<center><div class="social-share"></div></center>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js"></script>