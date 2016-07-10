#第1章 入门

>本章内容

>- Spring Boot简化Spring应用程序开发
- Spring Boot的基本特性
- Spring Boot工作区的设置

Spring Framework已有十余年的历史了，已成为Java应用程序开发框架的事实标准。在如此悠久的历史背景下，有人可能会认为Spring放慢了脚步，躺在了自己的荣誉簿上，再也做不出什么新鲜的东西，或者是让人激动的东西。甚至有人说，Spring是遗留项目，是时候去看看其他创新的东西了。

这些人说得不对。

Spring的生态圈里正在出现很多让人激动的新鲜事物，涉及的领域涵盖云计算、大数据、无模式的数据持久化、响应式编程以及客户端应用程序开发。

在过去的一年多时间里，最让人兴奋、回头率最高、最能改变游戏规则的东西，大概就是Spring Boot了。Spring Boot提供了一种新的编程范式，能在最小的阻力下开发Spring应用程序。有了它，你可以更加敏捷地开发Spring应用程序，专注于应用程序的功能，不用在Spring的配置上多花功夫，甚至完全不用配置。实际上，Spring Boot的一项重要工作就是让Spring不再成为你成功路上的绊脚石。

本书将探索Spring Boot开发的诸多方面，但在开始前，我们先大概了解一下Spring Boot的功能。

## 1.1 Spring风云再起

Spring诞生时是Java企业版（Java Enterprise Edition，JEE，也称J2EE）的轻量级代替品。无需开发重量级的Enterprise JavaBean（EJB），Spring为企业级Java开发提供了一种相对简单的方法，通过依赖注入和面向切面编程，用简单的Java对象（Plain Old Java Object，POJO）实现了EJB的功能。

虽然Spring的组件代码是轻量级的，但它的配置却是重量级的。一开始，Spring用XML配置，而且是很多XML配置。Spring 2.5引入了基于注解的组件扫描，这消除了大量针对应用程序自身组件的显式XML配置。Spring 3.0引入了基于Java的配置，这是一种类型安全的可重构配置方式，可以代替XML。

尽管如此，我们依旧没能逃脱配置的魔爪。开启某些Spring特性时，比如事务管理和Spring MVC，还是需要用XML或Java进行显式配置。启用第三方库时也需要显式配置，比如基于Thymeleaf的Web视图。配置Servlet和过滤器（比如Spring的`DispatcherServlet`）同样需要在web.xml或Servlet初始化代码里进行显式配置。组件扫描减少了配置量，Java配置让它看上去简洁不少，但Spring还是需要不少配置。

所有这些配置都代表了开发时的损耗。因为在思考Spring特性配置和解决业务问题之间需要进行思维切换，所以写配置挤占了写应用程序逻辑的时间。和所有框架一样，Spring实用，但与此同时它要求的回报也不少。

除此之外，项目的依赖管理也是件吃力不讨好的事情。决定项目里要用哪些库就已经够让人头痛的了，你还要知道这些库的哪个版本和其他库不会有冲突，这难题实在太棘手。

并且，依赖管理也是一种损耗，添加依赖不是写应用程序代码。一旦选错了依赖的版本，随之而来的不兼容问题毫无疑问会是生产力杀手。

Spring Boot让这一切成为了过去。

### 1.1.1 重新认识Spring

假设你受命用Spring开发一个简单的Hello World Web应用程序。你该做什么？我能想到一些基本的需要。

- 一个项目结构，其中有一个包含必要依赖的Maven或者Gradle构建文件；最起码要有Spring MVC和Servlet API这些依赖。
- 一个web.xml文件（或者一个`WebApplicationInitializer`实现），其中声明了Spring的`DispatcherServlet`。
- 一个启用了Spring MVC的Spring配置。
- 一个控制器类，以“Hello World”响应HTTP请求。
- 一个用于部署应用程序的Web应用服务器，比如Tomcat。

最让人难以接受的是，这份清单里只有一个东西是和Hello World功能相关的，即控制器，剩下的都是Spring开发的Web应用程序必需的通用样板。既然所有Spring Web应用程序都要用到它们，那为什么还要你来提供这些东西呢？

暂且假设控制器就是你所需要的一切，那么你会发现，代码清单1-1所示的基于Groovy的控制器类（虽然很简单）就是一个完整的Spring应用程序。

**代码清单1-1 一个完整的基于Groovy的Spring应用程序**
```
groovy
@RestController
class HelloController {
  @RequestMapping("/")
  def hello() {
    return "Hello World"
  }
}
```
这里没有配置，没有web.xml，没有构建说明，甚至没有应用服务器，但这就是整个应用程序了。Spring Boot会搞定执行应用程序所需的各种后勤工作，你只要搞定应用程序的代码就好。

假设你已经装好了Spring Boot的命令行界面（Command Line Interface，CLI），可以像下面这样在命令行里运行`HelloController`：
```
$ spring run HelloController.groovy
```
想必你已经注意到了，这里甚至没有编译代码，Spring Boot CLI可以运行未经编译的代码。

之所以选择用Groovy来写这个控制器示例，是因为Groovy语言的简洁与Spring Boot的简洁有异曲同工之妙。但Spring Boot并不强制要求使用Groovy。实际上，本书中的很多代码都是用Java写的，但在恰当的时候，偶尔也会出现一些Groovy代码。

不要客气，直接跳到1.2.1节吧，看看如何安装Spring Boot CLI，这样你就能试着编写这个小小的Web应用程序了。现在，你将看到Spring Boot的关键部分，看到它是如何改变Spring应用程序的开发方式的。

### 1.1.2 Spring Boot精要

Spring Boot将很多魔法带入了Spring应用程序的开发之中，其中最重要的是以下四个核心。

- *自动配置*：针对很多Spring应用程序常见的应用功能，Spring Boot能自动提供相关配置。
- *起步依赖*：告诉Spring Boot需要什么功能，它就能引入需要的库。
- *命令行界面*：这是Spring Boot的可选特性，借此你只需写代码就能完成完整的应用程序，无需传统项目构建。
- *Actuator*：让你能够深入运行中的Spring Boot应用程序，一探究竟。

每一个特性都在通过自己的方式简化Spring应用程序的开发。本书会探寻如何将它们发挥到极致，但就目前而言，先简单看看它们都提供了哪些功能吧。

####1.自动配置

在任何Spring应用程序的源代码里，你都会找到Java配置或XML配置（抑或两者皆有），它们为应用程序开启了特定的特性和功能。举个例子，如果你写过用JDBC访问关系型数据库的应用程序，那你一定在Spring应用程序上下文里配置过`JdbcTemplate`这个Bean。我打赌那段配置看起来是这样的：
```java
@Bean
public JdbcTemplate jdbcTemplate(DataSource dataSource) {
  return new JdbcTemplate(dataSource);
}
```
这段非常简单的Bean声明创建了一个`JdbcTemplate`的实例，注入了一个`DataSource`依赖。当然，这意味着你还需要配置一个`DataSource`的Bean，这样才能满足依赖。假设你将配置一个嵌入式H2数据库作为`DataSource` Bean，完成这个配置场景的代码大概是这样的：
```java
@Bean
public DataSource dataSource() {
  return new EmbeddedDatabaseBuilder()
          .setType(EmbeddedDatabaseType.H2)
          .addScripts('schema.sql', 'data.sql')
          .build();
}
```
这个Bean配置方法创建了一个嵌入式数据库，并指定在该数据库上执行两段SQL脚本。`build()`方法返回了一个指向该数据库的引用。

这两个Bean配置方法都不复杂，也不是很长，但它们只是典型Spring应用程序配置的一小部分。除此之外，还有无数Spring应用程序有着完全相同的方法。所有需要用到嵌入式数据库和`JdbcTemplate`的应用程序都会用到那些方法。简而言之，这就是一个样板配置。

既然它如此常见，那为什么还要你去写呢？

Spring Boot会为这些常见配置场景进行自动配置。如果Spring Boot在应用程序的Classpath里发现H2数据库的库，那么它就自动配置一个嵌入式H2数据库。如果在Classpath里发现`JdbcTemplate`，那么它还会为你配置一个`JdbcTemplate`的Bean。你无需操心那些Bean的配置，Spring Boot会做好准备，随时都能将其注入到你的Bean里。

Spring Boot的自动配置远不止嵌入式数据库和`JdbcTemplate`，它有大把的办法帮你减轻配置负担，这些自动配置涉及Java持久化API（Java Persistence API，JPA）、Thymeleaf模板、安全和Spring MVC。第2章会深入讨论自动配置这个话题。

####2.起步依赖

向项目中添加依赖是件富有挑战的事。你需要什么库？它的Group和Artifact是什么？你需要哪个版本？哪个版本不会和项目中的其他依赖发生冲突？

Spring Boot通过起步依赖为项目的依赖管理提供帮助。起步依赖其实就是特殊的Maven依赖和Gradle依赖，利用了传递依赖解析，把常用库聚合在一起，组成了几个为特定功能而定制的依赖。

举个例子，假设你正在用Spring MVC构造一个REST API，并将JSON（JavaScript Object Notation，JavaScript对象表示法）作为资源表述。此外，你还想运用遵循JSR-303规范的声明式校验，并使用嵌入式的Tomcat服务器来提供服务。要实现以上目标，你在Maven或Gradle里至少需要以下八个依赖：

- org.springframework:spring-core
- org.springframework:spring-web
- org.springframework:spring-webmvc
- com.fasterxml.jackson.core:jackson-databind
- org.hibernate:hibernate-validator
- org.apache.tomcat.embed:tomcat-embed-core
- org.apache.tomcat.embed:tomcat-embed-el
- org.apache.tomcat.embed:tomcat-embed-logging-juli

不过，如果打算利用Spring Boot的起步依赖，你只需添加Spring Boot的Web起步依赖（`org.springframework.boot:spring-boot-starter-web`）***{![Spring Boot的起步依赖基本都以`spring-boot-starter`打头，随后是直接代表其功能的名字，比如`web`、`test`，下文出现起步依赖的名字时，可能就直接用其前缀后的单词来表示了。——译者注]}***，仅此一个。它会根据依赖传递把其他所需依赖引入项目里，你都不用考虑它们。

比起减少依赖数量，起步依赖还引入了一些微妙的变化。向项目中添加了“web”起步依赖，实际上指定了应用程序所需的一类功能。因为应用是个Web应用程序，所以加入了“web”起步依赖。与之类似，如果应用程序要用到JPA持久化，那么就可以加入“jpa”起步依赖。如果需要安全功能，那就加入“security”起步依赖。简而言之，你不再需要考虑支持某种功能要用什么库了，引入相关起步依赖就行。

此外，Spring Boot的起步依赖还把你从“需要这些库的哪些版本”这个问题里解放了出来。起步依赖引入的库的版本都是经过测试的，因此你可以完全放心，它们之间不会出现不兼容的情况。

和自动配置一样，第2章就会深入讨论起步依赖。

####3.命令行界面

除了自动配置和起步依赖，Spring Boot还提供了一种很有意思的新方法，可以快速开发Spring应用程序。正如之前在1.1节里看到的那样，Spring Boot CLI让只写代码即可实现应用程序成为可能。

Spring Boot CLI利用了起步依赖和自动配置，让你专注于代码本身。不仅如此，你是否注意到代码清单1-1里没有`import`？CLI如何知道`RequestMapping`和`RestController`来自哪个包呢？说到这个问题，那些类最终又是怎么跑到Classpath里的呢？

说得简单一点，CLI能检测到你使用了哪些类，它知道要向Classpath中添加哪些起步依赖才能让它运转起来。一旦那些依赖出现在Classpath中，一系列自动配置就会接踵而来，确保启用`DispatcherServlet`和Spring MVC，这样控制器就能响应HTTP请求了。

Spring Boot CLI是Spring Boot的非必要组成部分。虽然它为Spring带来了惊人的力量，大大简化了开发，但也引入了一套不太常规的开发模型。要是这种开发模型与你的口味相去甚远，那也没关系，抛开CLI，你还是可以利用Spring Boot提供的其他东西。不过如果喜欢CLI，你一定想看看第5章，其中深入探讨了Spring Boot CLI。

####4.Actuator

Spring Boot的最后一块“拼图”是Actuator，其他几个部分旨在简化Spring开发，而Actuator则要提供在运行时检视应用程序内部情况的能力。安装了Actuator就能窥探应用程序的内部情况了，包括如下细节：

- Spring应用程序上下文里配置的Bean
- Spring Boot的自动配置做的决策
- 应用程序取到的环境变量、系统属性、配置属性和命令行参数
- 应用程序里线程的当前状态
- 应用程序最近处理过的HTTP请求的追踪情况
- 各种和内存用量、垃圾回收、Web请求以及数据源用量相关的指标

Actuator通过Web端点和shell界面向外界提供信息。如果要借助shell界面，你可以打开SSH（Secure Shell），登入运行中的应用程序，发送指令查看它的情况。

第7章会详细探索Actuator的功能。

### 1.1.3 Spring Boot不是什么

因为Spring Boot实在是太惊艳了，所以过去一年多的时间里有不少和它相关的言论。原先听到或看到的东西可能给你造成了一些误解，继续学习本书前应该先澄清这些误会。

首先，Spring Boot不是应用服务器。这个误解是这样产生的：Spring Boot可以把Web应用程序变为可自执行的JAR文件，不用部署到传统Java应用服务器里就能在命令行里运行。Spring Boot在应用程序里嵌入了一个Servlet容器（Tomcat、Jetty或Undertow），以此实现这一功能。但这是内嵌的Servlet容器提供的功能，不是Spring Boot实现的。

与之类似，Spring Boot也没有实现诸如JPA或JMS（Java Message Service，Java消息服务）之类的企业级Java规范。它的确支持不少企业级Java规范，但是要在Spring里自动配置支持那些特性的Bean。例如，Spring Boot没有实现JPA，不过它自动配置了某个JPA实现（比如Hibernate）的Bean，以此支持JPA。

最后，Spring Boot没有引入任何形式的代码生成，而是利用了Spring 4的条件化配置特性，以及Maven和Gradle提供的传递依赖解析，以此实现Spring应用程序上下文里的自动配置。

简而言之，从本质上来说，Spring Boot就是Spring，它做了那些没有它你自己也会去做的Spring Bean配置。谢天谢地，幸好有Spring，你不用再写这些样板配置了，可以专注于应用程序的逻辑，这些才是应用程序独一无二的东西。

现在，你应该对Spring Boot有了大概的认识，是时候构建你的第一个Spring Boot应用程序了。先从重要的事情开始，该怎么入手呢？

## 1.2 Spring Boot入门

从根本上来说，Spring Boot的项目只是普通的Spring项目，只是它们正好用到了Spring Boot的起步依赖和自动配置而已。因此，那些你早已熟悉的从头创建Spring项目的技术或工具，都能用于Spring Boot项目。然而，还是有一些简便的途径可以用来开启一个新的Spring Boot项目。

最快的方法就是安装Spring Boot CLI，安装后就可以开始写代码，如代码清单1-1，接着通过CLI来运行就好。

### 1.2.1 安装Spring Boot CLI

如前文所述，Spring Boot CLI提供了一种有趣的、不同寻常的Spring应用程序开发方式。第5章里会详细解释CLI提供的功能。这里先来看看如何安装Spring Boot CLI，这样才能运行代码清单1-1。

Spring Boot CLI有好几种安装方式：

- 用下载的分发包进行安装
- 用Groovy Environment Manager进行安装
- 通过OS X Homebrew进行安装
- 使用MacPorts进行安装

我们分别看一下这几种方式。除此之外，还要了解如何安装Spring Boot CLI的命令行补全支持，如果你在BASH或zsh shell里使用CLI，这会非常有用（抱歉了，各位Windows用户）。先来看看如何用分发包手工安装Spring Boot CLI吧。

####1.手工安装Spring Boot CLI

安装Spring Boot CLI最直接的方法大约是下载、解压，随后将它的bin目录添加到系统路径里。你可以从以下两个地址下载分发包：

- [http://repo.spring.io/release/org/springframework/boot/spring-boot-cli/1.3.0.RELEASE/spring-boot-cli-1.3.0.RELEASE-bin.zip](http://repo.spring.io/release/org/springframework/boot/spring-boot-cli/1.3.0.RELEASE/spring-boot-cli-1.3.0.RELEASE-bin.zip)
- [http://repo.spring.io/release/org/springframework/boot/spring-boot-cli/1.3.0.RELEASE/spring-boot-cli-1.3.0.RELEASE-bin.tar.gz](http://repo.spring.io/release/org/springframework/boot/spring-boot-cli/1.3.0.RELEASE/spring-boot-cli-1.3.0.RELEASE-bin.tar.gz)

下载完成之后，把它解压到文件系统的任意目录里。在解压后的目录里，你会找到一个bin目录，其中包含了一个spring.bat脚本（用于Windows环境）和一个spring脚本（用于Unix环境）。把这个bin目录添加到系统路径里，然后就能使用Spring Boot CLI了。

> __为Spring Boot建立符号链接__

> 如果是在安装了Unix的机器上使用Spring Boot CLI，最好建立一个指向解压目录的符号链接，然后把这个符号链接添加到系统路径，而不是实际的目录。这样后续升级Spring Boot新版本，或是转换版本，都会很方便，只要重建一下符号链接，指向新版本就好了。

你可以先浅尝辄止，看看你所安装的CLI版本号：
```
$ spring --version
```
如果一切正常，你会看到安装好的Spring Boot CLI的版本号。

虽然这是手工安装，但一切都很容易，而且不要求你安装任何附加的东西。如果你是Windows用户，也别无选择，这是唯一的安装方式。但如果你使用的是Unix机器，而且想要稍微自动化一点的方式，那么可以试试Software Development Kit Manager。

####2.使用Software Development Kit Manager进行安装

软件开发工具管理包（Software Development Kit Manager，SDKMAN，曾用简称GVM）也能用来安装和管理多版本Spring Boot CLI。使用前，你需要先从[http://sdkman.io](http://sdkman.io)获取并安装SDKMAN。最简单的安装方式是使用命令行：
```
$ curl -s get.sdkman.io | bash
```
跟随输出的指示就能完成SDKMAN的安装。在我的机器上，我在命令行里执行了如下命令：
```
$ source "/Users/habuma/.sdkman/bin/sdkman-init.sh"
```
请注意，用户不同，这条命令也会有所不同。因为我的用户目录是/Users/habuma，所以这也是shell脚本的根路径。你需要根据你的实际情况稍作调整。

一旦安装好了SDKMAN，就可以用下面的方式来安装Spring Boot CLI了：
```
$ sdk install springboot
$ spring --version
```
假设一切正常，你将看到Spring Boot的当前版本号。

如果想升级新版本的Spring Boot CLI，只需安装并使用即可。使用SDKMAN的`list`命令可以找到可用的版本：
```
$ sdk list springboot
```
`list`命令列出了所有可用版本，包括已经安装的和正在使用的。从中选择一个进行安装，然后就可以正常使用。举例来说，要安装Spring Boot CLI 1.3.0.RELEASE，直接使用`install`命令，指定版本号：
```
$ sdk install springboot 1.3.0.RELEASE
```
这样就会安装一个新版本，随后你会被询问是否将其设置为默认版本。要是你不想把它作为默认版本，或者想要切换到另一个版本，可以用`use`命令：
```
$ sdk use springboot 1.3.0.RELEASE
```
如果你希望把那个版本作为所有shell的默认版本，可以使用`default`命令：
```
$ sdk default springboot 1.3.0.RELEASE
```
使用SDKMAN来管理Spring Boot CLI有一个好处，你可以便捷地在Spring Boot的不同版本之间切换。这样你可以在正式发布前试用快照版本（snapshot）、里程碑版本（milestone）和尚未正式发布的候选版本（release candidate），试用后再切回稳定版本进行其他工作。

####3.使用Homebrew进行安装

如果要在OS X的机器上进行开发，你还可以用Homebrew来安装Spring Boot CLI。Homebrew是OS X的包管理器，用于安装多种不同应用程序和工具。要安装Homebrew，最简单的方法就是运行安装用的Ruby脚本：
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/ master/install)"
```
你可以在[http://brew.sh](http://brew.sh)看到更多关于Homebrew的内容（还有安装方法）。

要用Homebrew来安装Spring Boot CLI，你需要引入Pivotal的tap***{![tap是向Homebrew添加额外仓库的一种途径。Pivotal是Spring及Spring Boot背后的公司，通过它的tap可以安装Spring Boot。]}***：

```
$ brew tap pivotal/tap
```
在有了Pivotal的tap后，就可以像下面这样安装Spring Boot CLI了：
```
$ brew install springboot
```
Homebrew会把Spring Boot CLI安装到/usr/local/bin，之后可以直接使用。可以通过检查版本号来验证安装是否成功：
```
$ spring --version
```
这条命令应该会返回刚才安装的Spring Boot版本号。你也可以运行代码清单1-1看看。

####4.使用MacPorts进行安装

OS X用户还有另一种安装Spring Boot CLI的方法，即使用MacPorts，这是Mac OS X上另一个流行的安装工具。要使用MacPorts来安装Spring Boot CLI，必须先安装MacPorts，而MacPorts还要求安装Xcode。此外，使用不同版本的OS X时，MacPorts的安装步骤也会有所不同。因此我建议你根据[https://www.macports.org/install.php](https://www.macports.org/install.php)的安装指南来安装MacPorts。

一旦安装好了MacPorts，就可以用以下命令来安装Spring Boot CLI了：
```
$ sudo port install spring-boot-cli
```
MacPorts会把Spring Boot CLI安装到/opt/local/share/java/spring-boot-cli，并在/opt/local/bin里放一个指向其可执行文件的符号链接。在安装MacPorts后，/opt/local/bin这个目录应该就在系统路径里了。你可以检查版本号来验证安装是否成功：
```
$ spring --version
```
这条命令应该会返回刚才安装的Spring Boot的版本号。你也可以运行代码清单1-1看看。

####5.开启命令行补全

Spring Boot CLI为基于CLI的应用程序的运行、打包和测试提供了一套好用的命令。而且，每个命令都有好多选项。要记住这些东西实属不易，命令行补全能帮你记起怎么使用Spring Boot CLI。

如果用Homebrew安装Spring Boot CLI，那么命令行补全已经安装完毕。但如果是手工安装或者用SDKMAN安装的，那就需要执行脚本或者手工安装。（如果是通过MacPorts安装的Spring Boot CLI，那么你不必考虑命令行补全。）

你可以在Spring Boot CLI安装目录的shell-completion子目录里找到补全脚本。有两个不同的脚本，一个是针对BASH的，另一个是针对zsh的。要使用BASH的补全脚本，可以在命令行里键入以下命令（假设安装时用的是SDKMAN）：
```
$ . ~/.sdkman/springboot/current/shell-completion/bash/spring
```
这样，在当前的shell里就可以使用Spring Boot CLI的补全功能了，但每次开启一个新的shell都要重新执行一次上面的命令才行。你也可以把这个脚本复制到你的个人或系统脚本目录里，这个目录的位置在不同的Unix里也会有所不同，可以参考系统文档（或Google）了解细节。

开启了命令行补全之后，在命令行里键入`spring`命令，然后按Tab键就能看到下一步该输什么的提示。选中一个命令后，键入`--`（两个连字符）后再按Tab，就会显示出该命令的选项列表。

如果你在Windows上进行开发，或者没有用BASH或zsh，那就无缘使用这些命令行补全脚本了。尽管如此，如果你用的是Spring Boot CLI的shell，那一样也有命令补全：
```
$ spring shell
```
和BASH、zsh的命令补全脚本（在BASH/zsh shell里执行的）不同，Spring Boot CLI shell会新开一个特别针对Spring Boot的shell，在里面可以执行各种CLI命令，Tab键也能有命令补全。

Spring Boot CLI为Spring Boot提供了快速上手和构建简单原型应用程序的途径。稍后将在第8章中讲到，在正确的生产运行时环境下，它也能用于开发生产应用程序。

尽管如此，与大部分Java项目的开发相比，Spring Boot CLI的流程还是不太符合常规。通常情况下，Java项目用Gradle或Maven这样的工具构建出WAR文件，再把这些文件部署到应用服务器里。即便CLI模型让你感到不太舒服，你仍然可以在传统方式下充分利用大部分Spring Boot特性。***{![只是要放弃那些用到Groovy语言灵活性的特性，比如自动依赖和`import`解析。]}***Spring Initializr可以成为你万里长征的第一步。

###1.2.2 使用Spring Initializr初始化Spring Boot项目

万事开头难，你需要设置一个目录结构存放各种项目内容，创建构建文件，并在其中加入各种依赖。Spring Boot CLI消除了不少设置工作，但如果你更倾向于传统Java项目结构，那你应该看看Spring Initializr。

Spring Initializr从本质上来说就是一个Web应用程序，它能为你生成Spring Boot项目结构。虽然不能生成应用程序代码，但它能为你提供一个基本的项目结构，以及一个用于构建代码的Maven或Gradle构建说明文件。你只需要写应用程序的代码就好了。

Spring Initializr有几种用法：

- 通过Web界面使用
- 通过Spring Tool Suite使用
- 通过IntelliJ IDEA使用
- 使用Spring Boot CLI使用

下面分别看看这几种用法，先从Web界面开始。

####1.使用Spring Initializr的Web界面

要使用Spring Initializr，最直接的办法就是用浏览器打开[http://start.spring.io](http://start.spring.io)，你应该能看到类似图1-1的一个表单。

>P13 图1.1

__图1-1 Spring Initializr是生成空Spring项目的Web应用程序，可以视为开发过程的第一步__

表单的头两个问题是，你想用Maven还是Gradle来构建项目，以及使用Spring Boot的哪个版本。程序默认生成Maven项目，并使用Spring Boot的最新版本（非里程碑和快照版本），但你也可以自由选择其他选项。

表单左侧要你指定项目的一些基本信息。最起码你要提供项目的Group和Artifact，但如果你点击了“Switch to the full version”链接，还可以指定额外的信息，比如版本号和基础包名。这些信息是用来生成Maven的pom.xml文件（或者Gradle的build.gradle文件）的。

表单右侧要你指定项目依赖，最简单的方法就是在文本框里键入依赖的名称。随着你的输入会出现匹配依赖的列表，选中一个（或多个）依赖，选中的依赖就会加入项目。如果找不到你要的依赖，点击“Switch to the full version”就能看到可用依赖的完整列表。

要是你瞄过一眼附录B，就会发现这里的依赖和Spring Boot起步依赖是对应的。实际上，在这里选中依赖，就相当于告诉Initializr把对应的起步依赖加到项目的构建文件里。（第2章会进一步讨论Spring Boot起步依赖。）

填完表单，选好依赖，点击“Generate Project”按钮，Spring Initializr就会为你生成一个项目。浏览器将会以ZIP文件的形式（文件名取决于Artifact字段的内容）把这个项目下载下来。根据你的选择，ZIP文件的内容也会略有不同。不管怎样，ZIP文件都会包含一个极其基础的项目，让你能着手使用Spring Boot开发应用程序。

举例来说，假设你在Spring Initializr里指定了如下信息。

- Artifact：myapp
- 包名：myapp
- 类型：Gradle项目
- 依赖：Web和JPA

点击“Generate Project”，就能获得一个名为myapp.zip的ZIP文件。解压后的项目结构同图1-2类似。

>P14图

__图1-2 Initializr创建的项目，提供了构建Spring Boot应用程序所需的基本内容__

如你所见，项目里基本没有代码，除了几个空目录外，还包含了如下几样东西。

- build.gradle：Gradle构建说明文件。如果选择Maven项目，这就会换成pom.xml。
- `Application.java`：一个带有`main()`方法的类，用于引导启动应用程序。
- `ApplicationTests.java`：一个空的JUnit测试类，它加载了一个使用Spring Boot自动配置功能的Spring应用程序上下文。
- application.properties：一个空的properties文件，你可以根据需要添加配置属性。

在Spring Boot应用程序中，就连空目录都有自己的意义。static目录放置的是Web应用程序的静态内容（JavaScript、样式表、图片，等等）。还有，稍后你将看到，用于呈现模型数据的模板会放在templates目录里。

你很可能会把Initializr生成的项目导入IDE。如果你用的IDE是Spring Tool Suite，则可以直接在IDE里创建项目。下面来看看Spring Tool Suite是怎么创建Spring Boot项目的。

####2.在Spring Tool Suite里创建Spring Boot项目

长久以来，Spring Tool Suite***{![Spring Tool Suite是Eclipse IDE的一个发行版，增加了诸多能辅助Spring开发的特性。你可以从[http://spring.io/tools/sts](http://spring.io/tools/sts)下载Spring Tool Suite。]}***一直都是开发Spring应用程序的不二之选。从3.4.0版本开始，它就集成了Spring Initializr，这让它成为开始上手Spring Boot的好方法。

要在Spring Tool Suite里创建新的Spring Boot应用程序，在File菜单里选中New > Spring Starter Project菜单项，随后Spring Tool Suite会显示一个与图1-3相仿的对话框。

>P16图1.3

__图1-3 Spring Tool Suite集成了Spring Initializr，可以在IDE里创建并直接导入Spring Boot项目__

如你所见，这个对话框要求填写的信息和Spring Initializr的Web界面里是一样的。事实上，你在这里提供的数据会被发送给Spring Initializr，用于创建项目ZIP文件，这和使用Web表单是一样的。

如果你想在文件系统上指定项目创建的位置，或者把它加入IDE里的特定工作集，就点击Next按钮。你会看到第二个对话框，如图1-4所示。

>P16图1.4

__图1-4 Spring Starter Project对话框的第2页可以让你指定在哪里创建项目__

Location指定了文件系统上项目的存放位置。如果你使用Eclipse的工作集来组织项目，那么也可以勾上Add Project to Working Sets这个复选框，选择一个工作集，这样就能把项目加入指定的工作集了。

Site Info部分简单描述了将要用来访问Initializr的URL，大多数情况下你都可以忽略这部分内容。然而，如果要部署自己的Initializr服务器（从[https://github.com/spring-io/initializr](https://github.com/spring-io/initializr)复制代码即可），你可以在这里设置Initializr基础URL。

点击Finish按钮后，项目的生成和导入过程就开始了。你必须认识到一点，Spring Tool Suite的Spring Starter Project对话框，其实是把项目生成的工作委托给[http://start.spring.io](http://start.spring.io)上的Spring Initializr来做的，因此必须联网才能使用这一功能。

一旦把项目导入工作空间之后，就可以开发应用程序了。随着开发的进行，你会发现Spring Tool Suite针对Spring Boot还有一些锦上添花的功能。比如，可以在Run菜单里选中Run As > Spring Boot Application，在嵌入式服务器里运行你的应用程序。

注意，Spring Tool Suite是通过REST API与Initializr交互的，因此只有连上Initializr它才能正常工作。如果你的开发机离线，或者Initializr被防火墙阻断了，那么Spring Tool Suite的Spring Starter Project向导是无法使用的。

####3.在IntelliJ IDEA里创建Spring Boot项目

IntelliJ IDEA是非常流行的IDE，IntelliJ IDEA 14.1已经支持Spring Boot了！***{![你可以从[https://www.jetbrains.com/idea/](https://www.jetbrains.com/idea/)获取IntelliJ IDEA。它是一款商业IDE，这意味着你需要付费使用。但是你可以下载试用版，它对开源项目免费。]}***

要在IntelliJ IDEA里创建新的Spring Boot应用程序，在File菜单里选择New > Project。你会看到几屏内容（图1-5是第一屏），问的问题和Initializr的Web应用程序以及Spring Tool Suite类似。

>P17图

__图1-5 IntelliJ IDEA里Spring Boot初始化向导的第一屏__

在首先显示的这一屏中，在左侧项目选择里选中Spring Initializr，随后会提示你选择一个Project SDK（基本上就是这个项目要用的Java SDK），同时选择Initializr Web服务的位置。除非你在使用自己的Initializr，否则应该不做任何修改直接点Next按钮，之后就到了图1-6。

>P18图

__图1-6 在IntelliJ IDEA的Spring Boot初始化向导里指定项目信息__

Spring Boot初始化向导的第二屏要求你提供项目的一些基本信息，比如项目名称、Maven Group和Artifact、Java版本，以及你是想用Maven还是Gradle来构建项目。描述好项目信息之后，点击Next按钮就能看到第三屏了，如图1-7所示。

>P19图

__图1-7 在IntelliJ IDEA的Spring Boot初始化向导里选择项目依赖__

第二屏向你询问项目的基本信息，第三屏就开始问你要往项目里添加什么依赖了。和之前一样，屏幕里的复选框和Spring Boot起步依赖是对应的。选完之后点击Next就到了向导的最后一屏，如图1-8所示。

>P20图

__图1-8 IntelliJ IDEA的Spring Boot初始化向导的最后一屏__

最后一屏问你项目叫什么名字，还有要在哪里创建项目。一切准备就绪之后，点击Finish按钮，就能在IDE里得到一个空的Spring Boot项目了。

####4.在Spring Boot CLI里使用Initializr

如前文所述，如果你想仅仅写代码就完成Spring应用程序的开发，那么Spring Boot CLI是个不错的选择。然而，Spring Boot CLI的功能还不限于此，它有一些命令可以帮你使用Initializr，通过它上手开发更传统的Java项目。

Spring Boot CLI包含了一个`init`命令，可以作为Initializr的客户端界面。`init`命令最简单的用法就是创建Spring Boot项目的基线：
```
$ spring init
```
在和Initializr的Web应用程序通信后，`init`命令会下载一个demo.zip文件。解压后你会看到一个典型的项目结构，包含一个Maven的pom.xml构建描述文件。Maven的构建说明只包含最基本的内容，即只有Spring Boot基线和测试起步依赖。你可能会想要更多的东西。

假设你想要构建一个Web应用程序，其中使用JPA实现数据持久化，使用Spring Security进行安全加固，可以用`--dependencies`或`-d`来指定那些初始依赖：
```
$ spring init -dweb,jpa,security
```
这条命令会下载一个demo.zip文件，包含与之前一样的项目结构，但在pom.xml里增加了Spring Boot的web、jpa和security起步依赖。请注意，在`-d`和依赖之间不能加空格，否则就变成了下载一个ZIP文件，文件名是web,jpa,security。

现在，假设你想用Gradle来构建项目。没问题，用`--build`参数将Gradle指定为构建类型：
```
$ spring init -dweb,jpa,security --build gradle
```
默认情况下，无论是Maven还是Gradle的构建说明都会产生一个可执行JAR文件。但如果你想要一个WAR文件，那么可以通过`--packaging`或者`-p`参数进行说明：
```
$ spring init -dweb,jpa,security --build gradle -p war
```
到目前为止，`init`命令只用来下载ZIP文件。如果你想让CLI帮你解压那个ZIP文件，可以指定一个用于解压的目录：
```
$ spring init -dweb,jpa,security --build gradle -p war myapp
```
此处的最后一个参数说明你希望把项目解压到myapp目录里去。

此外，如果你希望CLI把生成的项目解压到当前目录，可以使用`--extract`或者`-x`参数：
```
$ spring init -dweb,jpa,security --build gradle -p jar -x
```
`init`命令还有不少其他参数，包括基于Groovy构建项目的参数、指定用Java版本编译的参数，还有选择构建依赖的Spring Boot版本的参数。可以通过`help`命令了解所有参数的情况：
```
$ spring help init
```
你也可以查看那些参数都有哪些可选项，为`init`命令带上`--list`或`-l`参数就可以了：
```
$ spring init -l
```
你一定注意到了，尽管`spring init -l`列出了一些Initializr支持的参数，但并非所有参数都能直接为Spring Boot CLI的`init`命令所支持。举例来说，用CLI初始化项目时，你不能指定根包的名字，它默认为demo。`spring help init`会告诉你CLI的`init`命令都支持哪些参数。

无论你是用Initializr的Web界面，在Spring Tool Suite里创建项目，还是用Spring Boot CLI来初始化项目，Spring Boot Initializr创建出来的项目都有相似的项目布局，和你之前开发过的Java项目没什么不同。

## 1.3 小结

Spring Boot为Spring应用程序的开发提供了一种激动人心的新方式，框架本身带来的阻力很小。自动配置消除了传统Spring应用程序里的很多样板配置；Spring Boot起步依赖让你能通过库所提供的功能而非名称与版本号来指定构建依赖；Spring Boot CLI将Spring Boot的无阻碍开发模型提升到了一个崭新的高度，在命令行里就能简单快速地用Groovy进行开发；Actuator让你能深入运行中的应用程序，了解Spring Boot做了什么，是怎么做的。

本章大致概括了Spring Boot的功能。你大概已经跃跃欲试，想用Spring Boot来写个真实的应用程序了吧。这正是我们在下一章里要做的事情。有了Spring Boot提供的诸多功能，最困难的不过是把书翻到第2章。
