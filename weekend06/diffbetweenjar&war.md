---
title: jar和war有什么区别？
date: 2019-04-27 13:38:22
categories:  #分类
    - ignorance
tags:   #标签
    - ARTS
    - tips 
description: 
    扩大认知，从根本上理解jar与war的区别
---
(https://www.ibm.com/developerworks/cn/java/)
## 什么是jar
在软件领域，jar文件（java archive)是一种软件包文件格式，通常用于聚合大量的java类文件，相关的元数据和资源文件到一个文件，以便java平台应用软件或库。jar文件是一种归档文件，以zip格式创建，以.jar为文件扩展名。用户可以使jdk自带的jar命令创建，或提取jar文件。也可以使用其他zip压缩工具，不过压缩时zip文件头里的条目顺序很重要，因为ManiFest文件常放在首位。jar文件内的文件名是Unicode文本。
### oracle对jar的定义
```
JAR stands for Java ARchive. It's a file format based on the popular ZIP file format and is used for aggregating many files into one. Although JAR can be used as a general archiving tool, the primary motivation for its development was so that Java applets and their requisite components (.class files, images and sounds) can be downloaded to a browser in a single HTTP transaction, rather than opening a new connection for each piece. This greatly improves the speed with which an applet can be loaded onto a web page and begin functioning. The JAR format also supports compression, which reduces the size of the file and improves download time still further. Additionally, individual entries in a JAR file may be `digitally signed` by the applet author to authenticate their origin.
JAR is :
  1.the only archive format that is cross-platform
  2.the only format that handles audio and image files as well as class files
  3.backward-compatible with existing applet code
  4.an open standard, fully extendable, and written in java
  5.the preferred way to bundle the pieces of a java applet
```
### 特性

JAR 文件格式提供了许多优势和功能，其中很多是传统的压缩格式如 ZIP 或者 TAR 所没有提供的。它们包括：
* 安全性。可以对 JAR 文件内容加上数字化签名。这样，能够识别签名的工具就可以有选择地为您授予软件安全特权，这是其他文件做不到的，它还可以检测代码是否被篡改过。(jarsigner工具使用 keystore 生成或者验证 JAR 文件的数字签名)
* 减少下载时间。如果一个 applet 捆绑到一个 JAR 文件中，那么浏览器就可以在一个 HTTP 事务中下载这个 applet 的类文件和相关的资源，而不是对每一个文件打开一个新连接。
* 压缩。JAR 格式允许您压缩文件以提高存储效率。
* 传输平台扩展。Java 扩展框架 (Java Extensions Framework) 提供了向 Java 核心平台添加功能的方法，这些扩展是用 JAR 文件打包的 (Java 3D 和 JavaMail 就是由 Sun 开发的扩展例子 )。
* 包密封。存储在 JAR 文件中的包可以选择进行 密封，以增强版本一致性和安全性。密封一个包意味着包中的所有类都必须在同一 JAR 文件中找到。
* 包版本控制。一个 JAR 文件可以包含有关它所包含的文件的数据，如厂商和版本信息。
* 可移植性。处理 JAR 文件的机制是 Java 平台核心 API 的标准部分。

### 如何生成
创建一个可执行 JAR 很容易。首先将所有应用程序代码放到一个目录中。假设应用程序中的主类是 com.mycompany.myapp.Sample。您要创建一个包含应用程序代码的 JAR 文件并标识出主类。为此，在某个位置 ( 不是在应用程序目录中 ) 创建一个名为 manifest的文件，并在其中加入以下一行：
```
Main-Class: com.mycompany.myapp.Sample
```
然后，像这样创建 JAR 文件：
```
jar cmf manifest ExecutableJar.jar application-dir
```
所要做的就是这些了 -- 现在可以用 java -jar执行这个 JAR 文件 ExecutableJar.jar。

一个可执行的 JAR 必须通过 menifest 文件的头引用它所需要的所有其他从属 JAR。如果使用了 -jar选项，那么环境变量 CLASSPATH 和在命令行中指定的所有类路径都被 JVM 所忽略。

### 有什么好处
可以对jar文件进行数字签名，方便下载,可扩展，还可以对JAR文件进行混淆

### 应用场景
可执行jar文件专门为windows系统的用户而设计的，通过双击可执行的jar文件（其实是基于jre中默认关联）就相当于执行了javaw -jar这个命令。Solaris 2.6内核也扩展了这个功能，通过执行java -jar这个命令
### 总结
JAR 格式远远超出了一种压缩格式，它有许多可以改进效率、安全性和组织 Java 应用程序的功能。因为这些功能已经建立在核心平台 -- 包括编译器和类装载器 -- 中了，所以开发人员可以利用 JAR 文件格式的能力简化和改进开发和部署过程。.

## 什么是war
Web应用程序归档，Web application archive也是一种jar文件，其中包含用来分发的JSP、Java Servlet、Java类、XML文件、标签库、静态网页（HTML和相关文件），以及构成Web应用程序的其他资源。一个WAR文件可能会以与JAR文件相同的方式进行数字签名，以便他人确定哪些源代码来自哪一个JAR文件。
而WAR文件也有其特殊的文件和目录。如果Web应用程序使用的servlet，则Servlet容器会使用web.xml文件，以确定某个URL请求将被路由到哪个Servlet上。web.xml还用于定义Servlet中可以引用的上下文变量，以及部署器所需配置的环境依赖关系。例如，一个依赖于邮件会话、用于发送电子邮件的程序，而Servlet容器负责提供这项服务。这就需要在web.xml进行一些配置。

### 特点
* 易于部署和测试
* 已部署的应用程序，其版本很容易辨别
* 所有的Java EE容器都支持.WAR文件
使用WAR文件进行Web部署的一个缺点是，即便是细微的修改，也不能在程序运行时进行。任何修改都需要重新生成和部署整个WAR文件。


### 应用场景
通常配合一些web容器（weblogic、jetty、tomcat、jboss等）进行部署，多用于单体工程的应用。

## jar和war的区别
Jar文件（扩展名为. Jar，Java Application Archive）包含Java类的普通库、资源（resources）、辅助文件（auxiliary files）等。通常是开发时要引用的通用类，打成包便于存放管理。简单来说，jar包就是别人已经写好的一些类，然后对这些类进行打包。可以将这些jar包引入到你的项目中，可以直接使用这些jar包中的类和属性，这些jar包一般放在lib目录下。

　　War文件（扩展名为.War,Web Application Archive）包含全部Web应用程序。在这种情形下，一个Web应用程序被定义为单独的一组文件、类和资源，用户可以对jar文件进行封装，并把它作为小型服务程序（servlet）来访问。 war包是一个可以直接运行的web模块，通常用于网站，打成包部署到容器中。以Tomcat来说，将war包放置在其\webapps\目录下,然后启动Tomcat，这个包就会自动解压，就相当于发布了。war包是Sun提出的一种web应用程序格式，与jar类似，是很多文件的压缩包。`war包中的文件按照一定目录结构来组织`。根据其根目录下包含有html和jsp文件，或者包含有这两种文件的目录，另外还有WEB-INF目录。通常在WEB-INF目录下含有一个web.xml文件和一个classes目录，web.xml是这个应用的配置文件，而classes目录下则包含编译好的servlet类和jsp，或者servlet所依赖的其他类（如JavaBean）。通常这些所依赖的类也可以打包成jar包放在WEB-INF下的lib目录下。