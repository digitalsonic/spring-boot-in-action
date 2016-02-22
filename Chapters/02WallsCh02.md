# Developing your first Spring Boot application
# 开发你的第一个Spring Boot应用程序

This chapter covers  
__本章内容涉及__

* Working with Spring Boot starters  
使用Spring Boot起步依赖
* Automatic Spring configuration  
自动进行Spring配置

When’s the last time you went to a supermarket or major retail store and actually had to push the door open? Most large stores have automatic doors that sense your presence and open for you. Any door will enable you to enter a building, but automatic doors don’t require that you push or pull them open.  
当你去超市或大型零售店，最后一次自己推开门是什么时候？大多数大型商店都安装了带感应功能的自动门，虽然所有门都能让你进入建筑物内，但自动门不用你去推开或者拉开它。

Similarly, many public facilities have restrooms with automatic water faucets and towel dispensers. Although not quite as prevalent as automatic supermarket doors, these devices don’t ask much of you and instead are happy to dispense water and towels.  
类似的，很多公共场所的卫生间里都装有自动水龙头和纸巾售卖机。虽然没有超市自动门这么普遍，但这些设施对你没有太多要求，能很方便地出水和纸巾。

And I honestly don’t remember the last time I even saw an ice tray, much less filled it with water or cracked it to get ice for a glass of water. My refrigerator/freezer somehow magically always has ice for me and is at the ready to fill a glass for me.  
说实话，我已经不记得上次看到制冰盒是什么时候了，更记不得往里面倒水制冰或者取冰了。我的冰箱就是这么神奇，总是有冰够我喝上一杯。

I bet you can think of countless ways that modern life is automated with devices that work for you, not the other way around. With all of this automation everywhere, you’d think that we’d see more of it in our development tasks. Strangely, that hasn’t been so.  
我敢打赌你也能想出无数例子，说明设备让现代化生活更自动化，而不是增加障碍。有了这些自动化的便利设施，你一定会想我们的开发任务里也应该看到更多类似的东西。但是很奇怪，事实并非如此。

Up until recently, creating an application with Spring required you to do a lot of work for the framework. Sure, Spring has long had fantastic features for developing amazing applications. But it was up to you to add all of the library dependencies to the project’s build specification. And it was your job to write configuration to tell Spring what to do.  
直到最近，用Spring创建应用程序还要求你为框架做很多事情。当然，Spring提供了很多优秀的特性，用于开发令人惊讶的应用程序。但是，你需要自己往项目的构建说明文件里添加各种库依赖；你还要自己写配置文件，告诉Spring做什么。

In this chapter, we’re going to look at two ways that Spring Boot has added a level of automation to Spring development: starter dependencies and automatic configuration. You’ll see how these essential Spring Boot features free you from the tedium and distraction of enabling Spring in your projects and let you focus on actually developing your applications. Along the way, you’ll write a small but complete Spring application that puts Spring Boot to work for you.  
Spring Boot将Spring开发的自动化程度提升到了一个新的高度，本章中我们会看到两种新方法：起步依赖和自动配置。在项目中启用Spring不仅枯燥乏味，还让人抓狂，你将看到这些基础的Spring Boot特性是如何将你解放出来的，它能让你集中精力开发自己的应用程序。与此同时，你会写一个很小的Spring应用程序，麻雀虽小，五脏俱全，其中会用上Spring Boot。

## 2.1 Putting Spring Boot to work
## 2.1 运用Spring Boot

The fact that you’re reading this tells me that you are a reader. Maybe you’re quite the bookworm, reading everything you can. Or maybe you only read on an as-needed basis, perhaps picking up this book only because you need to know how to develop applications with Spring.  
你正在阅读本书，说明你是一位读书人。也许你就是一个书虫，博览群书；也可能你只读自己需要的东西，拿起本书只是为了知道怎么用Spring开发应用程序。

Whatever the case may be, you’re a reader. And readers tend to maintain a reading list of books that they want (or need) to read. Even if it’s not a physical list, you probably have a mental list of things you’d like to read.1  
无论何种情况，你始终是一位读书人，读书人就想要维护一个阅读列表，里面是自己想读（或者需要读）的书。就算不是一个物理上的列表，至少在你心里会有这么一个列表。<sup>[1][]</sup>

Throughout this book, we’re going to build a simple reading-list application. With it, users can enter information about books they want to read, view the list, and remove books once they’ve been read. We’ll use Spring Boot to help us develop it quickly and with as little ceremony as possible.  
在本书中，我们会构建一个简单的阅读列表应用程序，在这个程序里，用户可以输入想读的图书信息，查看列表，删除已经读过的书。我们将使用Spring Boot来辅助快速开发，各种繁文缛节越少越好。

To start, we’ll need to initialize the project. In chapter 1, we looked at a handful of ways to use the Spring Initializr to kickstart Spring Boot development. Any of those choices will work fine here, so pick the one that suits you best and get ready to put Spring Boot to work.  
开始前，我们需要先初始化一个项目。在第1章里，我们看到了好几种用Spring Initializr来开始Spring Boot开发的方法，选择哪种都行，所以选个最合适的，然后开始着手用Spring Boot开发就好了。

From a technical standpoint, we’re going to use Spring MVC to handle web requests, Thymeleaf to define web views, and Spring Data JPA to persist the reading selections to a database. For now, that database will be an embedded H2 database. Although Groovy is an option, we’ll write the application code in Java for now. And we’ll use Gradle as our build tool of choice.  
从技术角度来看，我们要用Spring MVC来处理Web请求，用Thymeleaf来定义Web视图，用Spring Data JPA来把阅读列表持久化到数据库里，姑且先用嵌入式的H2数据库。虽然也可以用Groovy，我们还是先用Java来开发这个应用程序吧。此外，我们使用Gradle作为构建工具。

If you’re using the Initializr, either via its web application or through Spring Tool Suite or IntelliJ IDEA, you’ll want to be sure to select the check boxes for Web, Thymeleaf, and JPA. And also remember to check the H2 check box so that you’ll have an embedded database to use while developing the application.  
无论是用Web界面、Spring Tool Suite还是IntelliJ IDEA，只要是用了Initializr，你就要确保勾选了Web、Thymeleaf和JPA这几个复选框，还要记得勾上H2复选框，这样才能在开发应用程序时使用这个内嵌式数据库。

As for the project metadata, you’re welcome to choose whatever you like. For the purposes of the reading list example, however, I created the project with the information shown in figure 2.1.  
至于项目元信息，就随便你写了。以阅读列表为例，我创建项目时使用了图2.1中的信息。

[1]: # "If you’re not a reader, feel free to apply this to movies to watch, restaurants to try, or whatever suits you.要你不是一个读书人，就把书换成想看的电影、想去的餐厅，只要合适自己就好。"

![图2.1](../Figures/figure-2.1.png)

__Figure 2.1 Initializing the reading list app via Initializr’s web interface__  
__图2.1 通过Initializr的Web界面初始化阅读列表应用程序__

If you’re using Spring Tool Suite or IntelliJ IDEA to create the project, adapt the details in figure 2.1 for your IDE of choice.  
如果你创建项目时用的是Spring Tool Suite或者IntelliJ IDEA，把图2.1的内容适配成IDE需要的东西就好了。

On the other hand, if you’re using the Spring Boot CLI to initialize the application, you can enter the following at the command line:  
另一方面，如果是用Spring Boot CLI来初始化应用程序的，可以在命令行里键入以下内容：

```
$ spring init -dweb,data-jpa,h2,thymeleaf --build gradle readinglist
```

Remember that the CLI’s init command doesn’t let you specify the project’s root package or the project name. The package name will default to “demo” and the project name will default to “Demo”. After the project has been created, you’ll probably want to open it up and rename the “demo” package to “readinglist” and rename “DemoApplication.java” to “ReadingListApplication.java”.  
请记住，CLI的`init`命令是不能让你指定项目的根包名和项目名的，包名默认是“demo”，项目名默认是“Demo”。在项目创建完毕之后，你可以打开项目，把包名“demo”改为“readinglist”，把“DemoApplication.java”该名为“ReadingListApplication.java”。

![图2.2](../Figures/figure-2.2.png)

__Figure 2.2 The structure of the initialized readinglist project__  
__图2.2 初始化后的readinglist项目结构__

Once the project has been created, you should have a project structure similar to that shown in figure 2.2.  
项目创建完毕后，你应该能看到一个类似图2.2的项目结构。

This is essentially the same project structure as what the Initializr gave you in chapter 1. But now that you’re going to actually develop an application, let’s slow down and take a closer look at what’s contained in the initial project.  
这个项目结构基本上和第1章里Initializr生成的结构是一样的，只不过你现在是真的要去开发应用程序了，让我们先放慢脚步，仔细看看初始化出来的项目里都有什么东西。

### 2.1.1 Examining a newly initialized Spring Boot project
### 2.1.1 查看刚初始化的Spring Boot项目

The first thing to notice in figure 2.2 is that the project structure follows the layout of a typical Maven or Gradle project. That is, the main application code is placed in the src/main/java branch of the directory tree, resources are placed in the src/main/resources branch, and test code is placed in the src/test/java branch. At this point we don’t have any test resources, but if we did we’d put them in src/test/resources.  
图2.2中值得注意的第一件事是整个项目结构遵循传统Maven或Gradle项目的布局，即主要应用程序代码位于src/main/java目录里，资源都在src/main/resources目录里，测试代码则在src/test/java目录里。此刻我们还没有测试资源，但如果有的话要放在src/test/resources里。

Digging deeper, you’ll see a handful of files sprinkled about the project:  
再进一步，你会看到项目里还有不少文件：

* build.gradle—The Gradle build specification  
build.gradle——Gradle构建说明文件
* ReadingListApplication.java—The application’s bootstrap class and primary Spring configuration class  
ReadingListApplication.java——应用程序的启动引导类（bootstrap class），也是主要的Spring配置类
* application.properties—A place to configure application and Spring Boot properties  
application.properties——这里可以配置应用程序和Spring Boot的属性
* ReadingListApplicationTests.java—A basic integration test class  
ReadingListApplicationTests.java——一个基本的集成测试类

There’s a lot of Spring Boot goodness to uncover in the build specification, so I’ll save inspection of it until last. Instead, we’ll start with ReadingListApplication.java.  
构建说明文件里有很多Spring Boot的优点尚未揭秘，所以我打算把最好的留到最后，先让我们来看看ReadingListApplication.java。

#### BOOTSTRAPPING SPRING
#### 启动引导Spring

The ReadingListApplication class serves two purposes in a Spring Boot application: configuration and bootstrapping. First, it’s the central Spring configuration class. Even though Spring Boot auto-configuration eliminates the need for a lot of Spring configuration, you’ll need at least a small amount of Spring configuration to enable auto-configuration. As you can see in listing 2.1, there’s only one line of configuration code.  
`ReadingListApplication`在Spring Boot应用程序里有两个作用：配置和启动引导。首先，这是主要的Spring配置类，虽然Spring Boot的自动配置消除了很多Spring配置，但你还需要做一点点配置来启用自动配置。正如代码2.1所示，这里只有一行配置代码。

__Listing 2.1 ReadingListApplication.java is both a bootstrap class and a configuration class__  
__代码2.1 ReadingListApplication.java不仅是启动引导类，还是配置类__

```java
package readinglist;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ReadingListApplication {
  public static void main(String[] args) {
    SpringApplication.run(ReadingListApplication.class, args);
  }

}
```

Enable component-scanning and auto-configuration  
开启了组件扫描和自动配置

Bootstrap the application
负责启动引导应用程序

The @SpringBootApplication enables Spring component-scanning and Spring Boot auto-configuration. In fact, @SpringBootApplication combines three other useful annotations:  
`@SpringBootApplication`开启了Spring的组件扫描和Spring Boot的自动配置功能。实际上，`@SpringBootApplication`将三个有用的注解组合在了一起：

* Spring’s @Configuration—Designates a class as a configuration class using Spring’s Java-based configuration. Although we won’t be writing a lot of configuration in this book, we’ll favor Java-based configuration over XML configuration when we do.  
___Spring___的`@Configuration`——标明该类使用Spring的基于Java的配置。虽然本书中我们不会写太多配置，但在需要时我们会用基于Java的配置，而不是XML配置。
* Spring’s @ComponentScan—Enables component-scanning so that the web controller classes and other components you write will be automatically discovered and registered as beans in the Spring application context. A little later in this chapter, we’ll write a simple Spring MVC controller that will be annotated with @Controller so that component-scanning can find it.  
___Spring___的`@ComponentScan`——启用组件扫描，这样你写的Web控制器类和其他组件才会被自动发现并注册为Spring应用程序上下文里的Bean。本章稍后会写一个简单的Spring MVC控制器，使用了`@Controller`进行注解，这样组件扫描才能找到它。
* Spring Boot’s @EnableAutoConfiguration—This humble little annotation might as well be named @Abracadabra because it’s the one line of configuration that enables the magic of Spring Boot auto-configuration. This one line keeps you from having to write the pages of configuration that would be required otherwise.  
___Spring Boot___的`@EnableAutoConfiguration`——这个低调谦卑的注解也能被称为`@Abracadabra`<sup>[译注1][]</sup>，因为就这一行配置开启了Spring Boot自动配置的魔力，就这一行让你不用再写成篇的配置了。

In older versions of Spring Boot, you’d have annotated the ReadingListApplication class with all three of these annotations. But since Spring Boot 1.2.0, @SpringBootApplication is all you need.  
在Spring Boot的早期版本中，你需要在`ReadingListApplication`类上同时标上这三个注解，但从1.2.0开始，只用`@SpringBootApplication`就好了。

As I said, ReadingListApplication is also a bootstrap class. There are several ways to run Spring Boot applications, including traditional WAR file deployment. But for now the main() method here will enable you to run your application as an executable JAR file from the command line. It passes a reference to the ReadingListApplication class to SpringApplication.run(), along with the command-line arguments, to kick off the application.  
如我所说，`ReadingListApplication`还是一个启动引导类。要运行Spring Boot应用程序有几种方式，其中包含传统的WAR文件部署。但这里的`main()`方法让你可以在命令行里把该应用程序当做一个可执行JAR文件来运行。这里向`SpringApplication.run()`里传递了一个`ReadingListApplication`类的引用，还有命令行参数，通过这些东西启动了应用程序。

[译注1]: # "abracadabra的意思是咒语。"

In fact, even though you haven’t written any application code, you can still build the application at this point and try it out. The easiest way to build and run the application is to use the bootRun task with Gradle:  
实际上，就算一行代码也没写，此时你仍然可以构建应用程序尝尝鲜，要构建并运行应用程序，最简单的方法就是用Gradle的`bootRun`任务：

```
$ gradle bootRun
```

The bootRun task comes from Spring Boot’s Gradle plugin, which we’ll discuss more in section 2.12. Alternatively, you can build the project with Gradle and run it with java at the command line:  
`bootRun`任务来自于Spring Boot的Gradle插件，我们会在2.12节里详细讨论。此外，你也可以用Gradle构建项目，然后在命令行里用`java`来运行它：

```
$ gradle build
...
$ java -jar build/libs/readinglist-0.0.1-SNAPSHOT.jar
```

The application should start up fine and enable a Tomcat server listening on port 8080. You can point your browser at http://localhost:8080 if you want, but because you haven’t written a controller class yet, you’ll be met with an HTTP 404 (Not Found) error and an error page. Before this chapter is finished, though, that URL will serve your reading-list application.  
应用程序应该能正常运行，启动了一个监听8080端口的Tomcat服务器。要是愿意，你可以用浏览器访问[http://localhost:8080](http://localhost:8080)，但由于你还没写控制器类，你只会收到一个HTTP 404 (Not Found)错误，看到错误页。在本章结束前，这个URL将会提供一个阅读列表应用程序。

You’ll almost never need to change ReadingListApplication.java. If your application requires any additional Spring configuration beyond what Spring Boot auto-configuration provides, it’s usually best to write it into separate @Configuration- configured classes. (They’ll be picked up and used by component-scanning.) In exceptionally simple cases, though, you could add custom configuration to ReadingListApplication.java.  
你几乎不需要修改ReadingListApplication.java，如果你的应用程序需要Spring Boot自动配置以外的其他Spring配置，一般最好把它写到一个单独的`@Configuration`标注的类里。（组件扫描会发现这些类的。）在一些极度简单的情况下，可以把自定义配置加入ReadingListApplication.java里。

#### TESTING SPRING BOOT APPLICATIONS
#### 测试Spring Boot应用程序

The Initializr also gave you a skeleton test class to help you get started with writing tests for your application. But ReadingListApplicationTests (listing 2.2) is more than just a placeholder for tests—it also serves as an example of how to write tests for Spring Boot applications.  
Initializr还提供了一个测试类的骨架，可以基于它为你的应用程序编写测试。但`ReadingListApplicationTests`（代码2.2）不止是个测试的占位符，它还是一个例子，告诉你如何为Spring Boot应用程序编写测试。

___Listing 2.2 @SpringApplicationConfigurationloadsaSpringapplicationcontext___

```
package readinglist;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.SpringApplicationConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;
import readinglist.ReadingListApplication;

@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(
         classes = ReadingListApplication.class)
@WebAppConfiguration
public class ReadingListApplicationTests {

  @Test
  public void contextLoads() {
  }

}
```

Load context via Spring Boot  
通过Spring Boot加载上下文

Test that the context loads  
测试加载的上下文

In a typical Spring integration test, you’d annotate the test class with @ContextConfiguration to specify how the test should load the Spring application context. But in order to take full advantage of Spring Boot magic, the @SpringApplicationConfiguration annotation should be used instead. As you can see from listing 2.2, ReadingListApplicationTests is annotated with @SpringApplicationConfiguration to load the Spring application context from the ReadingListApplication configuration class.  
在一个典型的Spring集成测试里，会用`@ContextConfiguration`注解标识这个测试该如何加载Spring的应用程序上下文。但是，为了充分发挥Spring Boot的魔力，这里应该用`@SpringApplicationConfiguration`注解。正如你在代码2.2里看到的那样，`ReadingListApplicationTests`使用了`@SpringApplicationConfiguration`注解从`ReadingListApplication`配置类里加载Spring应用程序上下文。

ReadingListApplicationTests also includes one simple test method, contextLoads(). It’s so simple, in fact, that it’s an empty method. But it’s sufficient for the purpose of verifying that the application context loads without any problems. If the configuration defined in ReadingListApplication is good, the test will pass. If there are any problems, the test will fail.  
`ReadingListApplicationTests`里还有一个简单的测试方法，即`contextLoads()`。这个方法很简单，实际上它就是个空方法。但就这个空方法足以验证应用程序上下文的加载没有任何问题。如果`ReadingListApplication`里定义的配置是好的，就能通过测试。如果有问题，测试就会失败。

Of course, you’ll add some of your own tests as we flesh out the application. But the contextLoads() method is a fine start and verifies every bit of functionality provided by the application at this point. We’ll look more at how to test Spring Boot applications in chapter 4.  
当然，现在只是一个新的应用程序，你还会添加自己的测试。但`contextLoads()`方法是个良好的开端，此刻可以验证应用程序提供的各种功能。第4章里我们会更详细地讨论如何测试Spring Boot应用程序。

#### CONFIGURING APPLICATION PROPERTIES
#### 配置应用程序属性

The application.properties file given to you by the Initializr is initially empty. In fact, this file is completely optional, so you could remove it completely without impacting the application. But there’s also no harm in leaving it in place.  
Initializr为你生成的application.properties文件是一个空文件。实际上，这个文件完全是可选的，你大可以删掉它，不会对应用程序有任何影响，但留着也没什么问题。

We’ll definitely find opportunity to add entries to application.properties later. For now, however, if you want to poke around with application.properties, try adding the following line:  
稍后，我们肯定会有机会向application.properties里添加几个条目的。但现在，如果你想小试牛刀，可以加一行看看：

```
server.port=8000
```

With this line, you’re configuring the embedded Tomcat server to listen on port 8000 instead of the default port 8080. You can confirm this by running the application again.  
加上这一行，嵌入式Tomcat的监听端口就变成了8000，而不是默认的8080。你可以重新运行应用程序看看是不是这样。

This demonstrates that the application.properties file comes in handy for fine-grained configuration of the stuff that Spring Boot automatically configures. But you can also use it to specify properties used by application code. We’ll look at several examples of both uses of application.properties in chapter 3.  
这说明application.properties文件可以很方便地帮你细粒度地调整Spring Boot自动配置的东西。你还可以用它来指定应用程序代码所需的配置项。在第3章里我们会看到好几个例子，演示application.properties的这两种用法。

The main thing to notice is that at no point do you explicitly ask Spring Boot to load application.properties for you. By virtue of the fact that application.properties exists, it will be loaded and its properties made available for configuring both Spring and application code.  
要注意的是，你完全不用告诉Spring Boot为你加载application.properties，只要它存在就会被加载，Spring和应用程序代码都能获取到其中的属性。

We’re almost finished reviewing the contents of the initialized project. But we have
one last artifact to look at. Let’s see how a Spring Boot application is built.  
我们差不多已经把初始化的项目介绍完了，还剩最后一样东西，让我们来看看Spring Boot应用程序是如何构建的。

### 2.1.2 Dissecting a Spring Boot project build
### 2.1.2 Spring Boot项目构建过程解析

For the most part, a Spring Boot application isn’t much different from any Spring application, which isn’t much different from any Java application. Therefore, building a Spring Boot application is much like building any Java application. You have your choice of Gradle or Maven as the build tool, and you express build specifics much the same as you would in an application that doesn’t employ Spring Boot. But there are a few small details about working with Spring Boot that benefit from a little extra help in the build.  
Spring Boot应用程序大部分内容都与其他Spring应用程序没有什么区别，其实和其他Java应用程序也没什么两样，构建一个Spring Boot应用程序和构建其他Java应用程序的过程是类似的。你可以选择Gradle或Maven作为构建工具，描述构建说明文件的方法和你描述非Spring Boot应用程序时相同。但是，Spring Boot在构建过程中耍了些小把戏，在此需要做个小小的说明。

Spring Boot provides build plugins for both Gradle and Maven to assist in building Spring Boot projects. Listing 2.3 shows the build.gradle file created by Initializr, which applies the Spring Boot Gradle plugin.  
Spring Boot为Gradle和Maven提供了构建插件，以便辅助构建Spring Boot项目。代码2.3是Initializr创建的build.gradle文件，其中应用了Spring Boot的Gradle插件。

__Listing 2.3 Using the Spring Boot Gradle plugin__  
__代码2.3 使用Spring Boot的Gradle插件__

```
buildscript {
  ext {
    springBootVersion = `1.3.0.RELEASE`
  }
  repositories {
    mavenCentral()
  }
  dependencies {
    classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
  }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'idea'
apply plugin: 'spring-boot'

jar {
  baseName = 'readinglist'
  version = '0.0.1-SNAPSHOT'
}
sourceCompatibility = 1.7
targetCompatibility = 1.7

repositories {
  mavenCentral()
}

dependencies {
  compile("org.springframework.boot:spring-boot-starter-web")
  compile("org.springframework.boot:spring-boot-starter-data-jpa")
  compile("org.springframework.boot:spring-boot-starter-thymeleaf")
  runtime("com.h2database:h2")
  testCompile("org.springframework.boot:spring-boot-starter-test")
}

eclipse {
  classpath {
    containers.remove('org.eclipse.jdt.launching.JRE_CONTAINER')
    containers 'org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.
              ➥ debug.ui.launcher.StandardVMType/JavaSE-1.7'
  }
}
task wrapper(type: Wrapper) {
  gradleVersion = '1.12'
}
```

Depend on Spring Boot plugin  
依赖Spring Boot插件

Apply Spring Boot plugin  
运用Spring Boot插件

Starter dependencies  
起步依赖

On the other hand, had you chosen to build your project with Maven, the Initializr would have given you a pom.xml file that employs Spring Boot’s Maven plugin, as shown in listing 2.4.  
另一方面，要是你选择用Maven来构建你的应用程序，Initializr会替你生成一个pom.xml文件，其中使用了Spring Boot的Maven插件，如代码2.4所示。

__Listing 2.4 Using the Spring Boot Maven plugin and parent starter__  
__代码2.4 使用Spring Boot的Maven插件及父起步依赖__

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
      http://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>
  <groupId>com.manning</groupId>
  <artifactId>readinglist</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>ReadingList</name>
  <description>Reading List Demo</description>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>{springBootVersion}</version>
    <relativePath/> <!-- lookup parent from repository 在仓库里查找上一级 -->
  </parent>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <dependency>
      <groupId>com.h2database</groupId>
      <artifactId>h2</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <properties>
    <project.build.sourceEncoding>
    UTF-8
    </project.build.sourceEncoding>
    <start-class>readinglist.Application</start-class>
    <java.version>1.7</java.version>
  </properties>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>

</project>
```

Inherit versions from starter parent  
从spring-boot-starter-parent里继承版本号

Starter dependencies  
起步依赖

Apply Spring Boot plugin  
运用Spring Boot插件

Whether you choose Gradle or Maven, Spring Boot’s build plugins contribute to the build in two ways. First, you’ve already seen how you can use the bootRun task to run the application with Gradle. Similarly, the Spring Boot Maven plugin provides a spring-boot:run goal that achieves the same thing if you’re using a Maven build.  
无论你选择Gradle还是Maven，Spring Boot的构建插件都能对构建过程有所帮助。你已经看到过如何用Gradle的`bootRun`任务来运行应用程序了，Spring Boot的Maven插件与之类似地提供了一个`spring-boot:run`目标，如果你使用Maven，它能实现相同的功能。

The main feature of the build plugins is that they’re able to package the project as an executable uber-JAR. This includes packing all of the application’s dependencies within the JAR and adding a manifest to the JAR with entries that make it possible to run the application with java -jar.  
构建插件的主要功能是把项目打包成一个可执行的超级JAR（uber-JAR），包括把应用程序的所有依赖打入JAR文件内，并为JAR添加一个描述文件，其中的内容让你能用`java -jar`来运行应用程序。

In addition to the build plugins, notice that the Maven build in listing 2.4 has “spring-boot-starter-parent” as a parent. By rooting the project in the parent starter, the build can take advantage of Maven dependency management to inherit dependency versions for several commonly used libraries so that you don’t have to explicitly specify the versions when declaring dependencies. Notice that none of the <dependency> entries in this pom.xml file specify any versions.  
除了构建插件，代码2.4里的Maven构建说明中还将“spring-boot-starter-parent”作为上一级，这样一来就能利用Maven的依赖管理功能，继承很多常用库的依赖版本，在你声明依赖时就不用再去指定版本号了。请注意，这个pom.xml里的`<dependency>`都没有指定版本。

Unfortunately, Gradle doesn’t provide the same kind of dependency management as Maven. That’s why the Spring Boot Gradle plugin offers a third feature; it simulates dependency management for several common Spring and Spring-related dependencies. Consequently, the build.gradle file in listing 2.3 doesn’t specify any versions for any of its dependencies.  
不幸的是Gradle并没有Maven这样的依赖管理功能，为此Spring Boot Gradle插件提供了第三个特性，它为很多常用的Spring机器相关依赖模拟了依赖管理功能。其结果就是代码2.3的build.gradle里也没有为各项依赖指定版本。

Speaking of those dependencies, there are only five dependencies expressed in either build specification. And, with the exception of the H2 dependency you added manually, they all have artifact IDs that are curiously prefixed with “spring-boot-starter-”. These are Spring Boot starter dependencies, and they offer a bit of build-time magic for Spring Boot applications. Let’s see what benefit they provide.  
说起那些依赖，无论哪个构建说明文件里都只有五个依赖，除了你手工添加的H2之外，其他的Artifact ID都有“spring-boot-starter-”前缀。这些都是Spring Boot起步依赖，它们都有助于Spring Boot应用程序的构建。让我们来看看它们究竟都有哪些好处。

## 2.2 Using starter dependencies
## 2.2 使用起步依赖

To understand the benefit of Spring Boot starter dependencies, let’s pretend for a moment that they don’t exist. What kind of dependencies would you add to your build without Spring Boot? Which Spring dependencies do you need to support Spring MVC? Do you remember the group and artifact IDs for Thymeleaf? Which version of Spring Data JPA should you use? Are all of these compatible?  
要理解Spring Boot起步依赖带来的好处，就先让我们假设它们尚不存在。如果没用Spring Boot的话，你会向项目里添加哪些依赖呢？要用Spring MVC的话，你需要哪个Spring依赖？你还记得Thymeleaf的Group和Artifact ID么？你应该用哪个版本的Spring Data JPA呢？它们放在一起兼容么？

Uh-oh. Without Spring Boot starter dependencies, you’ve got some homework to do. All you want to do is develop a Spring web application with Thymeleaf views that persists its data via JPA. But before you can even write your first line of code, you have to go figure out what needs to be put into the build specification to support your plan.  
额，看起来如果没有Spring Boot起步依赖，你有不少作业要做。而你想要的只不过是开发一个Spring Web应用程序，使用Thymeleaf视图，通过JPA进行数据持久化。但在开始编写第一行代码之前，你要搞明白要支持你的计划，需要往构建说明里加入哪些东西。

After much consideration (and probably a lot of copy and paste from some other application’s build that has similar dependencies) you arrive at the following dependencies block in your Gradle build specification:  
考虑再三之后（也许你还从其他有相似依赖的应用程序构建说明中复制粘贴了不少内容），你的Gradle构建说明里大概会有下面这些东西：

```
compile("org.springframework:spring-web:4.1.6.RELEASE")
compile("org.thymeleaf:thymeleaf-spring4:2.1.4.RELEASE")
compile("org.springframework.data:spring-data-jpa:1.8.0.RELEASE")
compile("org.hibernate:hibernate-entitymanager:jar:4.3.8.Final")
compile("com.h2database:h2:1.4.187")
```

This dependency list is fine and might even work. But how do you know? What kind of assurance do you have that the versions you chose for those dependencies are even compatible with each other? They might be, but you won’t know until you build the application and run it. And how do you know that the list of dependencies is complete? With not a single line of code having been written, you’re still a long way from kicking the tires on your build.  
这段依赖列表是好的，而且应该也能正常工作，但你是怎么知道的？你怎么保证你选的这些版本能和其他东西兼容？也许可以，但在你构建并运行应用程序之前你是不知道的。再说了，你怎么知道这个列表是完整的？在还没写一行代码的情况下，你离开始构建还有很长的路要走呢。

Let’s take a step back and recall what it is we want to do. We’re looking to build an application with these traits:  
让我们退一步再想想，我们要做什么。我们要构建一个拥有如下功能的应用程序：

* It’s a web application  
这是一个Web应用程序
* It uses Thymeleaf  
它用了Thymeleaf
* It persists data to a relational database via Spring Data JPA  
它通过Spring Data JPA在关系型数据库里持久化数据

Wouldn’t it be simpler if we could just specify those facts in the build and let the build sort out what we need? That’s exactly what Spring Boot starter dependencies do.  
如果我们只在构建文件里指定这些功能，让构建过程自己搞明白我们要什么东西岂不是好？这正是Spring Boot起步依赖的功能。

### 2.2.1 Specifying facet-based dependencies
### 2.2.1 指定基于功能的依赖

Spring Boot addresses project dependency complexity by providing several dozen “starter” dependencies. A starter dependency is essentially a Maven POM that defines transitive dependencies on other libraries that together provide support for some functionality. Many of these starter dependencies are named to indicate the facet or kind of functionality they provide.  
Spring Boot通过提供众多“起步”依赖来降低项目依赖的复杂度。起步依赖本质上来说是一个Maven POM，定义了对其他库的传递依赖，这些东西加在一起即能支持某项功能。很多起步依赖的命名都暗示了它们提供的某种或某类功能。

For example, the reading-list application is going to be a web application. Rather than add several individually chosen library dependencies to the project build, it’s much easier to simply declare that this is a web application. You can do that by adding Spring Boot’s web starter to the build.  
举例来说，你打算把这个阅读列表应用程序做成一个Web应用程序，与其向项目的构建文件里添加一堆单独的库依赖，还不如声明这是一个Web应用程序来得简单。你只要添加Spring Boot的web起步依赖就好了。

We also want to use Thymeleaf for web views and persist data with JPA. Therefore, we need the Thymeleaf and Spring Data JPA starter dependencies in the build.  
我们还想用Thymeleaf来当Web视图，用JPA来实现数据持久化，因此在构建文件里还需要Thymeleaf和Spring Data JPA的起步依赖。

For testing purposes, we also want libraries that will enable us to run integration tests in the context of Spring Boot. Therefore, we also want a test-time dependency on Spring Boot’s test starter.  
为了能进行测试，我们还需要能在Spring Boot上下文里运行集成测试的库，因此要添加Spring Boot的test起步依赖，这时一个测试时依赖。

Taken altogether, we have the following five dependencies that the Initializr provided in the Gradle build:  
统统放在一起，我们就有了这五个依赖，也就是Initializr在Gradle的构建文件里提供的：

```
dependencies {
  compile "org.springframework.boot:spring-boot-starter-web"
  compile "org.springframework.boot:spring-boot-starter-thymeleaf"
  compile "org.springframework.boot:spring-boot-starter-data-jpa"
  compile "com.h2database:h2"
  testCompile("org.springframework.boot:spring-boot-starter-test")
}
```

As you saw earlier, the easiest way to get these dependencies into your application’s build is to select the Web, Thymeleaf, and JPA check boxes in the Initializr. But if you didn’t do that when initializing the project, you can certainly go back and add them later by editing the generated build.gradle or pom.xml.  
正如先前所见，添加这些依赖的最快方法就是在Initializr里选中Web、Thymeleaf和JPA复选框。但如果在初始化项目时没有这么做，当然也可以稍后再编辑生产的build.gradle或pom.xml。

Via transitive dependencies, adding these four dependencies is the equivalent of adding several dozen individual libraries to the build. Some of those transitive dependencies include such things as Spring MVC, Spring Data JPA, Thymeleaf, as well as any transitive dependencies that those dependencies declare.  
通过传递依赖，添加这四个依赖就等价于加了一大把独立的库，其中包含的东西有Spring MVC、Spring Data JPA、Thymeleaf等等，还有它们声明的依赖也会被传递依赖进来。

The most important thing to notice about the four starter dependencies is that they were only as specific as they needed to be. We didn’t say that we wanted Spring MVC; we simply said we wanted to build a web application. We didn’t specify JUnit or any other testing tools; we just said we wanted to test our code. The Thymeleaf and Spring Data JPA starters are a bit more specific, but only because there’s no less-specific way to declare that you want Thymeleaf and Spring Data JPA.  
最值得注意的是这四个起步依赖的具体程度恰到好处，我们并没有说我们想要Spring MVC，只是说我们想要构建一个Web应用程序。我们并没有指定JUnit或其他测试工具，只是说我们想要测试自己的代码。Thymeleaf和Spring Data JPA的起步依赖稍微具体一点，但这也只是因为没有更模糊的方法来声明这个需要了。

The four starters in this build are only a few of the many starter dependencies that Spring Boot offers. Appendix B lists all of the starters with some detail on what each one transitively brings to a project build.  
这四个起步依赖只是Spring Boot众多起步依赖中的沧海一粟。附录B罗列出了全部起步依赖，并稍微描述了一下它们传递依赖进了什么东西。

In no case did we need to specify the version. The versions of the starter dependencies themselves are determined by the version of Spring Boot you’re using. The starter dependencies themselves determine the versions of the various transitive dependencies that they pull in.  
我们并不需要指定版本号，起步依赖本身的版本是由正在使用的Spring Boot的版本来决定的，而起步依赖则会决定它们引入的传递依赖的版本。

Not knowing what versions of the various libraries are used may be a little unsettling to you. Be encouraged to know that Spring Boot has been tested to ensure that all of the dependencies pulled in are compatible with each other. It’s actually very liberating to just specify a starter dependency and not have to worry about which libraries and which versions of those libraries you need to maintain.  
不知道自己所用依赖的版本多少会让你有些不安，你要有信心，相信Spring Boot经过了足够的测试以确保引入的全部依赖都能相互兼容。这是一种解脱，只需指定起步依赖，不用担心自己需要维护哪些库以及它们的版本。

But if you really must know what it is that you’re getting, you can always get that from the build tool. In the case of Gradle, the dependencies task will give you a dependency tree that includes every library your project is using and their versions:  
但如果你真想知道自己在用的东西，构建工具里总能找到你要的答案。在Gradle里，`dependencies`任务会显示一个依赖树，其中包含了项目所用的每一个库以及它们的版本：

```
$ gradle dependencies
```

You can get a similar dependency tree from a Maven build with the tree goal of the
dependency plugin:  
在Maven里使用`dependency`插件的`tree`目标也能获得相似的依赖树。

```
$ mvn dependency:tree
```

For the most part, you should never concern yourself with the specifics of what each Spring Boot starter dependency provides. Generally, it’s enough to know that the web starter enables you to build a web application, the Thymeleaf starter enables you to use Thymeleaf templates, and the Spring Data JPA starter enables data persistence to a database using Spring Data JPA.  
大部分情况下，你都无需关心每个Spring Boot起步依赖都声明了些什么东西。web起步依赖让你能构建Web应用程序，Thymeleaf起步依赖让你能用Thymeleaf模板，Spring Data JPA起步依赖让你能用Spring Data JPA将数据持久化到数据库里，通常只要知道这些就足够了。

But what if, in spite of the testing performed by the Spring Boot team, there’s a problem with a starter dependency’s choice of libraries? How can you override the starter?  
但是，即使经过了Spring Boot团队的测试，起步依赖里所选的库仍有问题该怎么办？如何覆盖起步依赖呢？

### 2.2.2 Overriding starter transitive dependencies
### 2.2.2 覆盖起步依赖引入的传递依赖

Ultimately, starter dependencies are just dependencies like any other dependency in your build. That means you can use the facilities of the build tool to selectively override transitive dependency versions, exclude transitive dependencies, and certainly specify dependencies for libraries not covered by Spring Boot starters.  
说到底，起步依赖和你项目里的其他依赖没什么区别。也就是说，你可以通过构建工具中的功能，选择性地覆盖它们引入的传递依赖的版本号，排除传递依赖，当然还可以指定那些Spring Boot起步依赖没有涵盖到的库。

For example, consider Spring Boot’s web starter. Among other things, the web starter transitively depends on the Jackson JSON library. This library is handy if you’re building a REST service that consumes or produces JSON resource representations. But if you’re using Spring Boot to build a more traditional human-facing web application, you may not need Jackson. Even though it shouldn’t hurt anything to include it, you can trim the fat off of your build by excluding Jackson as a transitive dependency.  
以Spring Boot的web起步依赖为例，它传递依赖了Jackson JSON库，如果你正在构建一个生产或消费JSON资源表述的REST服务，那它会很有用的。但要是你正在构建的是一个传统的面向人类用户的Web应用程序，你可能就用不上Jackson了。就算把它加进来也不会有什么坏处，但排除掉它可以为你的项目瘦身。

If you’re using Gradle, you can exclude transitive dependencies like this:  
如果你在用Gradle，可以像这样来排除传递依赖：

```
compile("org.springframework.boot:spring-boot-starter-web") {
  exclude group: 'com.fasterxml.jackson.core'
}
```

In Maven, you can exclude transitive dependencies with the <exclusions> element. The following <dependency> for the Spring Boot web starter has <exclusions> to keep Jackson out of the build:  
在Maven里，可以用`<exclusions>`元素来排除传递依赖，下面这个引入Spring Boot的web起步依赖的`<dependency>`增加了`<exclusions>`元素来去除Jackson：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <groupId>com.fasterxml.jackson.core</groupId>
    </exclusion>
  </exclusions>
</dependency>
```

On the other hand, maybe having Jackson in the build is fine, but you want to build against a different version of Jackson than what the web starter references. Suppose that the web starter references Jackson version 2.3.4, but you’d rather user version 2.4.3.2 Using Maven, you can express the desired dependency directly in your project’s pom.xml file like this:  
另一方面，也许项目里需要Jackson，但你需要用另一个版本的Jackson来进行构建，而不是web起步依赖里的那个。假设web起步依赖引用了Jackson 2.3.4，但你需要使用2.4.3<sup>[2][]</sup>。在Maven里，你可以直接在项目的pom.xml里表达你的诉求，就像这样：

```
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.4.3</version>
</dependency>
```

Maven always favors the closest dependency, meaning that because you’ve expressed this dependency in your project’s build, it will be favored over the one that’s transitively referred to by another dependency.  
Maven总是会用最近的依赖，也就是说你在项目的构建说明文件里增加了这个依赖，它会覆盖传递依赖引入的另一个依赖。

Similarly, if you’re building with Gradle, you can specify the newer version of Jackson in your build.gradle file like this:  
类似的，如果你用的是Gradle，可以在build.gradle文件里指明你要的Jackson的版本：

```
compile("com.fasterxml.jackson.core:jackson-databind:2.4.3")
```

This dependency works in Gradle because it’s newer than the version transitively referred to by Spring Boot’s web starter. But suppose that instead of using a newer version of Jackson, you’d like to use an older version. Unlike Maven, Gradle favors the newest version of a dependency. Therefore, if you want to use an older version of Jackson, you’ll have to express the older version as a dependency in your build and exclude it from being transitively resolved by the web starter dependency:


[2]: # "The versions mentioned here are for illustration purposes only. The actual version of Jackson referenced by Spring Boot’s web starter will be determined by which version of Spring Boot you are using."


```
compile("org.springframework.boot:spring-boot-starter-web") {
  exclude group: 'com.fasterxml.jackson.core'
}
compile("com.fasterxml.jackson.core:jackson-databind:2.3.1")
```

In any case, take caution when overriding the dependencies that are pulled in transitively by Spring Boot starter dependencies. Although different versions may work fine, there’s a great amount of comfort that can be taken knowing that the versions chosen by the starters have been tested to play well together. You should only override these transitive dependencies under special circumstances (such as a bug fix in a newer version).

Now that we have an empty project structure and build specification ready, it’s time to start developing the application itself. As we do, we’ll let Spring Boot handle the configuration details while we focus on writing the code that provides the reading-list functionality.
