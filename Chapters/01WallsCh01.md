# Bootstarting Spring
# Spring Boot入门

This chapter covers  
__本章内容涉及__

* How Spring Boot simplifies Spring application development  
Spring Boot是如何简化Spring应用程序开发的
* The essential features of Spring Boot  
Spring Boot的基本特性
* Setting up a Spring Boot workspace  
设置Spring Boot工作区

The Spring Framework has been around for over a decade and has found a place as the de facto standard framework for developing Java applications. With such a long and storied history, some might think that Spring has settled, resting on its laurels, and is not doing anything new or exciting. Some might even say that Spring is legacy and that it’s time to look elsewhere for innovation.  
Spring Framework已经有超过十年的历史了，它已然成为了Java应用程序开发框架的事实标准。在如此悠久的历史背景下，有人可能会认为Spring放慢了脚步，躺在了自己的荣誉簿上，再也做不出什么新鲜的东西，或者是让人激动的东西了。更有甚者已经把Spring说成了遗留项目，是时候去看看其他创新的东西了。

Some would be wrong.  
这些人说错了。

There are many exciting new things taking place in the Spring ecosystem, including work in the areas of cloud computing, big data, schema-less data persistence, reactive programming, and client-side application development.  
在Spring的生态圈里出现了很多让人激动的新鲜事物，涉及的领域涵盖云计算、大数据、无模式的数据持久化、响应式编程以及客户端应用程序开发。

Perhaps the most exciting, most head-turning, most game-changing new thing to come to Spring in the past year or so is Spring Boot. Spring Boot offers a new paradigm for developing Spring applications with minimal friction. With Spring Boot, you’ll be able to develop Spring applications with more agility and be able to focus on addressing your application’s functionality needs with minimal (or possibly no) thought of configuring Spring itself. In fact, one of the main things that Spring Boot does is to get Spring out of your way so you can get stuff done.  
在过去的一年多时间里，最让人兴奋的、回头率最高的、最能改变游戏规则的东西大概就是Spring Boot了。Spring Boot提供了一种新的编程范式，让你能在最小的阻力下开发Spring应用程序。有了它，你就可以更加敏捷地开发Spring应用程序，专注于应用程序自己的功能，不用在Spring的配置上多花功夫（也可能是完全不用配置）。实际上，Spring Boot的一项重要工作就是让Spring不再成为你成功路上的绊脚石。

Throughout the chapters in this book, we’ll explore various facets of Spring Boot development. But first, let’s take a high-level look at what Spring Boot has to offer.  
全书通篇，我们会看到Spring Boot开发的诸多方面，但在开始前，我们先概要性地了解一下Spring Boot所提供的东西。

## 1.1 Spring rebooted
## 1.1 Spring风云再起

Spring started as a lightweight alternative to Java Enterprise Edition (JEE, or J2EE as it was known at the time). Rather than develop components as heavyweight Enterprise JavaBeans (EJBs), Spring offered a simpler approach to enterprise Java development, utilizing dependency injection and aspect-oriented programming to achieve the capabilities of EJB with plain old Java objects (POJOs).  
Spring诞生时是作为Java Enterprise Edition（当时称为JEE或J2EE）的轻量级代替品。无需开发重量级的Enterprise JavaBeans（EJBs）组件，Spring为企业级Java开发提供了一种相对简单的方法，通过依赖注入和面向切面编程用POJO（plain old Java object）实现了EJB的功能。

But while Spring was lightweight in terms of component code, it was heavyweight in terms of configuration. Initially, Spring was configured with XML (and lots of it). Spring 2.5 introduced annotation-based component-scanning, which eliminated a great deal of explicit XML configuration for an application’s own components. And Spring 3.0 introduced a Java-based configuration as a type-safe and refactorable option to XML.  
虽然Spring的组件代码很轻量级，但它的配置却很重量级。一开始，Spring是用XML配置的（而且是很多XML配置）。Spring 2.5引入了基于注解的组件扫描，这消除了大量针对应用程序自身组件的显式XML配置。在Spring 3.0里引入了基于Java的配置，这是一种类型安全、可重构的配置方式，可以代替XML。

Even so, there was no escape from configuration. Enabling certain Spring features such as transaction management and Spring MVC required explicit configuration, either in XML or Java. Enabling third-party library features such as Thymeleaf-based web views required explicit configuration. Configuring servlets and filters (such as Spring’s DispatcherServlet) required explicit configuration in web.xml or in a servlet initializer. Component-scanning reduced configuration and Java configuration made it less awkward, but Spring still required a lot of configuration.  
尽管如此，我们还是没能逃脱配置的魔爪。开启某些Spring特性时还是需要用XML或Java进行显式的配置，比如事务管理和Spring MVC。启用第三方库时也需要显式配置，比如基于Thymeleaf的Web视图。配置Servlet和过滤器（比如Spring的`DispatcherServlet`）也需要在web.xml或Servlet初始化代码里进行显式配置。组件扫描减少了配置量，Java配置让它看起来不这么难看了，但Spring还是需要不少配置。

All of that configuration represents development friction. Any time spent writing configuration is time spent not writing application logic. The mental shift required to think about configuring a Spring feature distracts from solving the business problem. Like any framework, Spring does a lot for you, but it demands that you do a lot for it in return.  
所有的这些配置都代表了开发时遭遇的损耗，花时间写配置就浪费了写应用程序逻辑的时间。你需要在思考如何配置Spring特性和如何解决业务问题之间进行思维切换。和任何框架一样，Spring为你做了很多事情，但与此同时它也要求你做不少事以示回报。

Moreover, project dependency management is a thankless task. Deciding what libraries need to be part of the project build is tricky enough. But it’s even more challenging to know which versions of those libraries will play well with others.  
除此之外，项目的依赖管理也是件吃力不讨好的事情。决定项目里要用哪些库就已经够让人头痛的了，你还要知道这些库的哪个版本和其他库不会有冲突，这件事的挑战实在是太大了。

As important as it is, dependency management is another form of friction. When you’re adding dependencies to your build, you’re not writing application code. Any incompatibilities that come from selecting the wrong versions of those dependencies can be a real productivity killer.  
依赖管理也是一种损耗，当你添加依赖时，你没有在写应用程序代码。一旦选错了依赖的版本，随之而来的不兼容问题毫无疑问会是生产力杀手。

Spring Boot has changed all of that.  
Spring Boot让这一切成为了过去。

### 1.1.1 Taking a fresh look at Spring
### 1.1.1 重新认识Spring

Suppose you’re given the task of developing a very simple Hello World web application with Spring. What would you need to do? I can think of a handful of things you’d need at a bare minimum:  
假设你受命用Spring开发一个简单的Hello World Web应用程序。你该做什么？我能想到一些你需要的最基本的东西：

* A project structure, complete with a Maven or Gradle build file including required dependencies. At the very least, you’ll need Spring MVC and the Servlet API expressed as dependencies.  
一个项目结构，其中有一个包含必要依赖的Maven或者Gradle构建文件。最起码，你要有Spring MVC和Servlet API这些依赖。
* A web.xml file (or a WebApplicationInitializer implementation) that declares Spring’s DispatcherServlet.  
一个web.xml（或者一个`WebApplicationInitializer`实现），其中声明了Spring的`DispatcherServlet`。
* A Spring configuration that enables Spring MVC.  
一个启用了Spring MVC的Spring配置。
* A controller class that will respond to HTTP requests with “Hello World”.  
一个控制器类，它会用“Hello World”来响应HTTP请求。
* A web application server, such as Tomcat, to deploy the application to.  
一个用于部署应用程序的Web应用服务器，比如Tomcat。

What’s most striking about this list is that only one item is specific to developing the Hello World functionality: the controller. The rest of it is generic boilerplate that you’d need for any web application developed with Spring. But if all Spring web applications need it, why should you have to provide it?  
最让人难以接受的是这份清单里只有一个东西是和你所开发的Hello World功能相关的，即控制器，剩下的都是那种所有用Spring开发的Web应用程序都需要的通用样板。既然所有Spring Web应用程序都要用到它们，那为什么还要你来提供这些东西呢？

Suppose for a moment that the controller is all you need. As it turns out, the Groovy-based controller class shown in listing 1.1 is a complete (even if simple) Spring application.  
暂且假设控制器就是你所需要的一切，那么你会发现，代码1.1所示的基于Groovy的控制器类（虽然很简单）就是一个完整的Spring应用程序。

Listing 1.1 A complete Groovy-based Spring application  
__代码1.1 一个完整的基于Groovy的Spring应用程序__

```groovy
@RestController
class HelloController {
  @RequestMapping("/")
  def hello() {
    return "Hello World"
  }
}
```

There’s no configuration. No web.xml. No build specification. Not even an application server. This is the entire application. Spring Boot will handle the logistics of executing the application. You only need to bring the application code.  
这里没有配置，没有web.xml，没有构建说明，甚至都没有应用服务器，但这就是整个应用程序了。Spring Boot会搞定执行应用程序所需的各种后勤工作的，你只要搞定应用程序的代码就好了。

Assuming that you have Spring Boot’s command-line interface (CLI) installed, you can run HelloController at the command line like this:  
假设你已经装好了Spring Boot的命令行界面（CLI），可以像下面这样在命令行里运行`HelloController`：

```
$ spring run HelloController.groovy
```

You may have also noticed that it wasn’t even necessary to compile the code. The Spring Boot CLI was able to run it from its uncompiled form.  
你应该已经注意到了，你甚至都没有编译代码，Spring Boot CLI可以运行未经编译的代码。

I chose to write this example controller in Groovy because the simplicity of the Groovy language presents well alongside the simplicity of Spring Boot. But Spring Boot doesn’t require that you use Groovy. In fact, much of the code we’ll write in this book will be in Java. But there’ll be some Groovy here and there, where appropriate.  
我之所以选择用Groovy来写这个范例控制器，是因为Groovy语言的简单性与Spring Boot的简单性有异曲同工之妙。但Spring Boot并不强制你使用Groovy。实际上，本书中的很多代码都是用Java写的，但在恰当的时候，偶尔也会出现一些Groovy代码。

Feel free to look ahead to section 1.21 to see how to install the Spring Boot CLI, so that you can try out this little web application. But for now, we’ll look at the key pieces of Spring Boot to see how it changes Spring application development.  
不要客气，直接跳到1.21小节吧，看看如何安装Spring Boot CLI，这样你就能试试这个小小的Web应用程序了。现在，我们将看到Spring Boot的关键部分，看看它是如何改变Spring应用程序的开发方式的。
