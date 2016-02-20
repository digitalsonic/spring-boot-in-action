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

The first thing to notice in figure 2.2 is that the project structure follows the layout of a typical Maven or Gradle project. That is, the main application code is placed in the src/main/java branch of the directory tree, resources are placed in the src/main/resources branch, and test code is placed in the src/test/java branch. At this point we don’t have any test resources, but if we did we’d put them in src/test/resources.

Digging deeper, you’ll see a handful of files sprinkled about the project:

* build.gradle—The Gradle build specification
* ReadingListApplication.java—The application’s bootstrap class and primary Spring configuration class
* application.properties—A place to configure application and Spring Boot properties
* ReadingListApplicationTests.java—A basic integration test class

There’s a lot of Spring Boot goodness to uncover in the build specification, so I’ll save inspection of it until last. Instead, we’ll start with ReadingListApplication.java.

#### BOOTSTRAPPING SPRING

The ReadingListApplication class serves two purposes in a Spring Boot application: configuration and bootstrapping. First, it’s the central Spring configuration class. Even though Spring Boot auto-configuration eliminates the need for a lot of Spring configuration, you’ll need at least a small amount of Spring configuration to enable auto-configuration. As you can see in listing 2.1, there’s only one line of configuration code.

__Listing 2.1 ReadingListApplication.java is both a bootstrap class and a configuration class__

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

Bootstrap the application

The @SpringBootApplication enables Spring component-scanning and Spring Boot auto-configuration. In fact, @SpringBootApplication combines three other useful annotations:

* Spring’s @Configuration—Designates a class as a configuration class using Spring’s Java-based configuration. Although we won’t be writing a lot of configuration in this book, we’ll favor Java-based configuration over XML configuration when we do.
* Spring’s @ComponentScan—Enables component-scanning so that the web controller classes and other components you write will be automatically discovered and registered as beans in the Spring application context. A little later in this chapter, we’ll write a simple Spring MVC controller that will be annotated with @Controller so that component-scanning can find it.
* Spring Boot’s @EnableAutoConfiguration—This humble little annotation might as well be named @Abracadabra because it’s the one line of configuration that enables the magic of Spring Boot auto-configuration. This one line keeps you from having to write the pages of configuration that would be required otherwise.

In older versions of Spring Boot, you’d have annotated the ReadingListApplication class with all three of these annotations. But since Spring Boot 1.2.0, @SpringBootApplication is all you need.

As I said, ReadingListApplication is also a bootstrap class. There are several ways to run Spring Boot applications, including traditional WAR file deployment. But for now the main() method here will enable you to run your application as an executable JAR file from the command line. It passes a reference to the ReadingListApplication class to SpringApplication.run(), along with the command-line arguments, to kick off the application.
