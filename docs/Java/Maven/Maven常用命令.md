[TOC]

## 1. 常用命令

Maven 中的一些常见命令如下：

| 命令          | 含义 | 功能                                           |
| ------------- | ---- | ---------------------------------------------- |
| `mvn clean`   | 清理 | 用于清理已编译好的文件                         |
| `mvn compile` | 编译 | 将 Java 源代码编译成字节码 `.class` 文件       |
| `mvn test`    | 测试 | 项目测试                                       |
| `mvn package` | 打包 | 根据用户配置，将项目打包为 `jar` 包或 `war` 包 |
| `mvn install` | 安装 | 手动向本地仓库安装一个 `jar`                   |
| `mvn deploy`  | 上传 | 将 `jar` 上传到私服                            |

## 2. 利用 Archetype 来生成项目骨架

实际上，为了更快捷的创建 Maven 项目骨架，我们可以使用 maven archetype 来创建，创建过程如下：

1.  首先进入你要创建项目骨架的目录，然后执行如下命令：

```shell
mvn archetype:generate
```

2.  接着会有很长的输出，最后又多种可用的 Archetype 供你选择，选择你所需要的，然后输入对应编号；

![](https://s1.ax1x.com/2020/07/07/UFZGW9.png)

3.  接着会让你输入 `groupId`、`artifactId`、`version`、`package` 等信息；

![](https://s1.ax1x.com/2020/07/07/UFZJzR.png)

4.  接着让你确认相关信息；

![](https://s1.ax1x.com/2020/07/07/UFZ8JJ.png)

5.  最后确认无误后，回车生成即可。

## 3. 项目结构

项目生成后的目录中主要包含如下文件：

![](https://s1.ax1x.com/2020/07/06/UFCr79.png)

其中 `src` 目录包含了项目的主代码和资源，同时还包括了测试相关的代码以及资源。而 `pom.xml` 则定义了项目的所有配置。

![](https://s1.ax1x.com/2020/07/06/UFPl36.png)

![](https://s1.ax1x.com/2020/07/06/UFCXjS.png)

![](https://gitee.com/cunyu1943/images/raw/master/ImgsUbuntu/20200510234310.png)

---
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/css/share.min.css">
<center><div class="social-share"></div></center>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js"></script>