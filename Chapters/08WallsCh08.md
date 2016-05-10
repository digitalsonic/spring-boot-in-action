# Deploying Spring Boot applications

__This chapter covers__
__本章内容涉及__

* Deploying WAR files
* Database migration
* Deploying to the cloud
* 部署WAR文件
* 数据库迁移
* 部署到云端

Think of your favorite action movie. Now imagine going to see that movie in the theater and being taken on a thrilling audio-visual ride with high-speed chases, explosions, and battles, only to have it come to a sudden end just before the good guys take down the bad guys. Instead of seeing the movie’s conflict resolved, the theater lights come on and everyone is ushered out the door.  
想一想你喜欢的动作电影，现在假设你要去电影院看这部电影，试听感受震撼，片中有高速追逐、爆炸和激战，最后好人战胜了坏人前一切戛然而止。还没等影片里的冲突得到解决，电影院的灯亮了，大家都被领出门外了。

Although the lead-up was exciting, it’s the climax of the movie that’s important. Without it, it’s action for action’s sake.
虽然前面的铺垫很精彩，但电影的高潮才是最重要的。没有了它，就是为了动作而动作了。

Now imagine developing applications and putting a lot of effort and creativity into solving the business problem, but then never deploying the application for others to use and enjoy. Sure, most applications we write don’t involve car chases or explosions (at least I hope not), but there’s a certain rush we get along the way. Of course, not every line of code we write is destined for production, but it’d be a big letdown if none of it ever was deployed.  
现在，想象你正在开发应用程序，为了解决某个业务问题投入了很多精力和创造力，但最终没能部署应用程序让别人来使用和享受它。我们写的大多数应用程序没有汽车追逐和爆炸（至少我希望是这样的），但一路上我们也会抢时间。当然，也不是每行代码都是为生产环境而写的，但什么都不部署也挺让人失望的。

Up to this point we’ve been focused on using features of Spring Boot that help us develop an application. There have been some exciting steps along the way. But it’s all for nothing if we don’t cross the finish line and deploy the application.  
到目前为止，我们的焦点都集中在使用Spring Boot的特性来帮助大家开发应用程序，其中有不少让人惊喜的地方。但如果我们不越过终点线，没有部署应用程序，这一切都是徒劳的。

In this chapter we’re going to step beyond developing applications with Spring Boot and look at how to deploy those applications. Although this may seem obvious for anyone who has ever deployed a Java-based application, there are some unique features of Spring Boot and related Spring projects we can draw on that make deploying Spring Boot applications unique.  
在本章中，我们会在使用Spring Boot开发应用程序的基础上更进一步，讨论如何部署那些应用程序。虽然这对那些部署过基于Java的应用程序的人来说很普通，但Spring Boot和相关的Spring项目中有些独特的功能，基于这些功能我们可以看让Spring Boot应用程序的部署变得与众不同。

In fact, unlike most Java web applications, which are typically deployed to an application server as WAR files, Spring Boot offers several deployment options. Before we look at how to deploy a Spring Boot application, let’s consider all of the options and choose a few that suit our needs best.  
实际上，大部分Java Web应用程序都是以WAR文件的形式部署到应用服务器上的，Spring Boot提供的部署方式则有所不同。在了解如何部署Spring Boot应用程序之前，让我们先来看看有哪些可选方式能满足我们的需求。

## 8.1 Weighing deployment options
## 8.1 衡量多种部署方式

There are several ways to build and run Spring Boot applications. You’ve already seen a few of them:

* Running the application in the IDE (either Spring ToolSuite or IntelliJ IDEA)
* Running from the command line using the Maven spring-boot:run goal or Gradle bootRun task
* Using Maven or Gradle to produce an executable JAR file that can be run at the command line
* Using the Spring Boot CLI to run Groovy scripts at the command line
* Using the Spring Boot CLI to produce an executable JAR file that can be run at the command line

Any of these choices is suitable for running the application while you’re still developing it. But what about when you’re ready to deploy the application into a production or other non-development environment?

Although none of the choices listed seems fitting for deploying an application beyond development, the truth is that all but one of them is a valid choice. Running an application within the IDE is certainly ill-suited for a production deployment. Executable JAR files and the Spring Boot CLI, however, are still on the table and are great choices when deploying to a cloud environment.

That said, you’re probably wondering how to deploy a Spring Boot application to a more traditional application server environment such as Tomcat, WebSphere, or WebLogic. In those cases, executable JAR files and Groovy source code won’t work. For application server deployment, you’ll need your application wrapped up in a WAR file.

As it turns out, Spring Boot applications can be packaged for deployment in several ways, as described in table 8.1.

__Table 8.1 Spring Boot deployment choices__

| Deployment artifact | Produced by                       | Target environment                                                                                           |
|---------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| Raw Groovy source   | Written by hand                   | Cloud Foundry and container deployment, such as with Docker                                                  |
| Executable JAR      | Maven, Gradle, or Spring Boot CLI | Cloud environments, including Cloud Foundry and Heroku, as well as container deployment, such as with Docker |
| WAR                 | Maven or Gradle                   | Java application servers or cloud environments such as Cloud Foundry                                         |

As you can see in table 8.1, your target environment will need to be a factor in your choice. If you’re deploying to a Tomcat server running in your own data center, then the choice of a WAR file has been made for you. On the other hand, if you’ll be deploying to Cloud Foundry, you’re welcome to choose any of the deployment options shown.

In this chapter, we’re going to focus our attention on the following options:

* Deploying a WAR file to a Java application server
* Deploying an executable JAR file to Cloud Foundry
* Deploying an executable JAR file to Heroku (where the build is performed by Heroku)

As we explore these scenarios, we’re also going to have to deal with the fact that we’ve been using an embedded H2 database as we’ve developed the application, and we’ll look at ways to replace it with a production-ready database.

To get started, let’s take a look at how we can build our reading-list application into a WAR file that can be deployed to a Java application server such as Tomcat, WebSphere, or WebLogic.
