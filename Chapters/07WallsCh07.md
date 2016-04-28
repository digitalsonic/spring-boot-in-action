# Taking a peek inside with the Actuator
# 深入Actuator

__This chapter covers__
__本章内容涉及__

* Actuator web endpoints
* Adjusting the Actuator
* Shelling into a running application
* Securing the Actuator
* Actuator Web端点
* 调整Actuator
* 通过Shell连入运行中的应用程序
* 保护Actuator

Have you ever tried to guess what’s inside a wrapped gift? You shake it, weigh it, and measure it. And you might even have a solid idea as to what’s inside. But until you open it up, there’s no way of knowing for sure.  
你有没有猜过包好的礼物盒里装的是什么东西？你会摇一摇，掂一掂，量一量，你甚至会执着于里面到底有什么。但直到打开盒子那一刻前，你是没办法确认里面是什么的。

A running application is kind of like a wrapped gift. You can poke at it and make reasonable guesses as to what’s going on under the covers. But how can you know for sure? If only there were some way that you could peek inside a running application, see how it’s behaving, check on its health, and maybe even trigger operations that influence how it runs?  
运行中的应用程序就像礼物盒。你可以刺探它，作出合理的推测，猜测它的运行情况。但如何了解真实的情况呢？有没有一种办法能让你深入应用程序内部一窥究竟，了解它的行为，检查它的健康状况，甚至触发一些操作来影响应用程序呢？

In this chapter, we’re going to explore Spring Boot’s Actuator. The Actuator offers production-ready features such as monitoring and metrics to Spring Boot applications. The Actuator’s features are provided by way of several REST endpoints, a remote shell, and Java Management Extensions (JMX). We’ll start by looking at the Actuator’s REST endpoints, which offer the most complete and well-known way of working with the Actuator.  
在本章中，我们将了解到Spring Boot的Actuator，它提供了很多生产级的特性，比如监控和度量Spring Boot应用程序。可以通过众多REST端点、远程Shell和Java Management Extensions（JMX）来获得Actuator的这些特性。我们会先来看看Actuator的REST端点，这种最为人所知的使用方式提供的功能也最完整。

## 7.1 Exploring the Actuator’s endpoints
## 7.1 揭秘Actuator的端点

The key feature of Spring Boot’s Actuator is that it provides several web endpoints in your application through which you can view the internals of your running application. Through the Actuator, you can find out how beans are wired together in the Spring application context, determine what environment properties are available to your application, get a snapshot of runtime metrics, and more.  
Spring Boot Actuator的关键特性是在应用程序里提供了众多Web端点，通过它们可以了解应用程序运行时的内部状况。有了Actuator，你可以了解到Bean在Spring应用程序上下文里是如何组装在一起的，掌握应用程序可以获取的环境属性信息，获取运行时度量信息的快照……

The Actuator offers a baker’s dozen of endpoints, as described in table 7.1.  
Actuator提供了13个端点，具体如表7.1所示。

__Table 7.1 Actuator endpoints__
__表7.1 Actuator的端点__

| HTTP method  | Path            | Description                                                                                           |
|--------------|-----------------|-------------------------------------------------------------------------------------------------------|
|GET           | /autoconfig     | Provides an auto-configuration report describing what autoconfiguration conditions passed and failed. |
|GET           | /configprops    | Describes how beans have been injected with configuration properties (including default values).      |
|GET           | /beans          | Describes all beans in the application context and their relationship to each other.                  |
|GET           | /dump           | Retrieves a snapshot dump of thread activity.                                                         |
|GET           | /env            | Retrieves all environment properties.                                                                 |
|GET           | /env/{name}     | Retrieves a specific environment value by name.                                                       |
|GET           | /health         | Reports health metrics for the application, as provided by HealthIndicator implementations.           |
|GET           | /info           | Retrieves custom information about the application, as provided by any properties prefixed with info. |
|GET           | /mappings       | Describes all URI paths and how they’re mapped to controllers (including Actuator endpoints).         |
|GET           | /metrics        | Reports various application metrics such as memory usage and HTTP request counters.                   |
|GET           | /metrics/{name} | Reports an individual application metric by name.                                                     |
|GET           | /shutdown       | Shuts down the application; requires that endpoints.shutdown.enabled be set to true.                  |
|GET           | /trace          | Provides basic trace information (timestamp, headers, and so on) for HTTP requests.                   |

| HTTP方法 | 路径              | 描述                                                           |
|----------|-------------------|---------------------------------------------------------------|
| GET      | `/autoconfig`     | 提供了一份自动配置报告，记录了哪些自动配置条件通过了，哪些没通过。  |
| GET      | `/configprops`    | 描述了配置属性（包含默认值）是如何注入到Bean里的。                |
| GET      | `/beans`          | 描述了应用程序上下文里全部的Bean，以及它们的相互关系。             |
| GET      | `/dump`           | 获取线程活动的快照。                                            |
| GET      | `/env`            | 获取全部环境属性。                                              |
| GET      | `/env/{name}`     | 根据名称获取特定的环境属性值。                                   |
| GET      | `/health`         | 报告应用程序的健康指标，这些值由`HealthIndicator`的实现类提供。    |
| GET      | `/info`           | 获取应用程序的定制信息，这些信息由`info`打头的属性提供。           |
| GET      | `/mappings`       | 描述全部的URI路径，以及它们和控制器的映射关系（包含Actuator端点）。 |
| GET      | `/metrics`        | 报告各种应用程序度量信息，比如内存用量和HTTP请求计数。             |
| GET      | `/metrics/{name}` | 报告指定名称的应用程序度量值。                                    |
| GET      | `/shutdown`       | 关闭应用程序，要求`endpoints.shutdown.enabled`设置为`true`。     |
| GET      | `/trace`          | 提供基本的HTTP请求跟踪信息（时间戳、HTTP头等）。                   |

To enable the Actuator endpoints, all you must do is add the Actuator starter to your build. In a Gradle build specification, that dependency looks like this:  
要启用Actuator的端点，只需在项目中引入Actuator的起步依赖即可。在Gradle构建说明文件里，这个依赖是这样的：

```
compile 'org.springframework.boot:spring-boot-starter-actuator'
```

For a Maven build, the required dependency is as follows:  
对于Maven项目，引入的依赖是这样的：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Or, if you’re using the Spring Boot CLI, the following @Grab should do the trick:  
亦或者，如果你在用Spring Boot CLI，可以使用如下`@Grab`注解：

```
@Grab('spring-boot-starter-actuator')
```

No matter which technique you use to add the Actuator to your build, auto-configuration will kick in when the application is running and you enable the Actuator.  
无论你是如何添加Actuator的，在应用程序运行时自动配置都会生效，开启Actuator。

The endpoints in table 7.1 can be organized into three distinct categories: configuration endpoints, metrics endpoints, and miscellaneous endpoints. Let’s take a look at each of these endpoints, starting with the endpoints that provide insight into the configuration of your application.  
表7.1中的端点可以分为三大类：配置端点、度量端点和其他端点。让我们分别了解一下这些端点，从提供应用程序配置信息的端点看起吧。

### 7.1.1 Viewing configuration details
### 7.1.1 查看配置明细

One of the most common complaints lodged against Spring component-scanning and autowiring is that it’s hard to see how all of the components in an application are wired together. Spring Boot auto-configuration makes this problem even worse, as there’s even less Spring configuration. At least with explicit configuration, you could look at the XML file or the configuration class and get an idea of the relationships between the beans in the Spring application context.  
关于Spring组件扫描和自动织入抱怨最多的几点之一就是很难看到应用程序中的组件是如何装配起来的。Spring Boot自动配置让这个问题变得更糟糕了，因为Spring的配置更少了。在有显式配置的情况下，至少你还能看到XML文件或者配置类，对Spring应用程序上下文里的Bean关系有个大概的了解。

Personally, I’ve never had this concern. Maybe it’s because I realize that before Spring came along there wasn’t any map of the components in my applications.  
从个人角度来说，我从不担心这个问题。也许是因为我意识到了在Spring出现之前，根本就没有应用程序组件的映射关系。

Nevertheless, if it concerns you that auto-configuration hides how beans are wired up in the Spring application context, then I have some good news! The Actuator has endpoints that give you that missing application component map as well as some insight into the decisions that auto-configuration made when populating the Spring application context.  
但是，如果你担心自动配置隐藏了Spring应用程序上下文中Bean的装配细节，那么我告诉你一个好消息！Actuator有一些端点不仅可以显示组件映射关系，还可以告诉你自动配置在配置Spring应用程序上下文时做了哪些决策。

#### GETTING A BEAN WIRING REPORT
#### 获得Bean装配报告

The most essential endpoint for exploring an application’s Spring context is the /beans endpoint. This endpoint returns a JSON document describing every single bean in the application context, its Java type, and any of the other beans it’s injected with. By performing a GET request to /beans (http://localhost:8080/beans when running locally), you’ll be given information similar to what’s shown in the following listing.  
要了解应用程序中Spring上下文的情况，最重要的端点就是`/beans`，它会返回一个JSON文档，描述了上下文里每个Bean的情况，它的Java类型以及注入的其他Bean。当向`/beans`（在本地运行时是[http://localhost:8080/beans](http://localhost:8080/beans)）发起GET请求后，你会看到与如下代码示例类似的信息。

__Listing 7.1 The /beans endpoint exposes the beans in the Spring application context__  
__代码7.1 `/beans`端点提供的Spring应用程序上下文Bean信息__

```
[
  {
    "beans": [
      {
        "bean": "application",
        "dependencies": [],
        "resource": "null",
        "scope": "singleton",
        "type": "readinglist.Application$$EnhancerBySpringCGLIB$$f363c202"
      },
      {
        "bean": "amazonProperties",
        "dependencies": [],
        "resource": "URL [jar:file:/../readinglist-0.0.1-SNAPSHOT.jar!
                                      /readinglist/AmazonProperties.class]",
        "scope": "singleton",
        "type": "readinglist.AmazonProperties"
      },
      {
        "bean": "readingListController",
        "dependencies": [
          "readingListRepository",
          "amazonProperties"
        ],
        "resource": "URL [jar:file:/../readinglist-0.0.1-SNAPSHOT.jar!
                               /readinglist/ReadingListController.class]",
        "scope": "singleton",
        "type": "readinglist.ReadingListController"
      },
      {
        "bean": "readerRepository",
        "dependencies": [
          "(inner bean)#219df4f5",
          "(inner bean)#2c0e7419",
          "(inner bean)#7d86037b",
          "jpaMappingContext"
        ],
        "resource": "null",
        "scope": "singleton",
        "type": "readinglist.ReaderRepository"
      },
      {
        "bean": "readingListRepository",
        "dependencies": [
          "(inner bean)#98ce66",
          "(inner bean)#1fd7add0",
          "(inner bean)#59faabb2",
          "jpaMappingContext"
        ],
        "resource": "null",
        "scope": "singleton",
        "type": "readinglist.ReadingListRepository"
      },
      ...
    ],
    "context": "application",
    "parent": null
  }
]
```

Bean ID  
Bean ID

Resource file  
资源文件

Dependencies  
依赖

Bean scope  
Bean作用域

Java type  
Java类型

Listing 7.1 is an abridged listing of the beans from the reading-list application. As you can see, all of the bean entries carry five pieces of information about the bean:  
代码7.1是阅读列表应用程序Bean信息的一个片段，如你所见，所有的Bean条目都由五类信息：

* bean—The name or ID of the bean in the Spring application context
* resource—The location of the physical .class file (often a URL into the built JAR file, but this might vary depending on how the application is built and run)
* dependencies—A list of bean IDs that this bean is injected with
* scope—The bean’s scope (usually singleton, as that is the default scope)
* type—The bean’s Java type
* `bean`——Spring应用程序上下文中的Bean名称或ID
* `resource`——.class文件的物理位置（通常是一个URL，指向构建出的JAR文件，这会随着应用程序的构建和运行方式发生变化）
* `dependencies`——当前Bean注入的Bean ID列表
* `scope`——Bean的作用域（通常是单例，这也是默认作用域）
* `type`——Bean的Java类型

Although the beans report doesn’t draw a specific picture of how the beans are wired together (for example, via properties or constructor arguments), it does help you visualize the relationships of the beans in the application context. Indeed, it would be reasonably easy to write a utility that processes the beans report and produces a graphical representation of the bean relationships. Be aware, however, that the full bean report includes many beans, including many auto-configured beans, so such a graphic could be quite busy.  
虽然Bean报告不会用图告诉你Bean是如何装配的（例如，通过属性或构造方法），但它帮你直观地了解了应用程序上下文中的Bean关系。实际上，很容易就能写出一个工具，把Bean报告处理一下，用图形化的方式来展现Bean关系。请注意，完整的Bean报告会包含很多Bean，还有很多自动配置的Bean，自动画出来的图会非常复杂。

#### EXPLAINING AUTO-CONFIGURATION
#### 详解自动配置

Whereas the /beans endpoint produces a report telling you what beans are in the Spring application context, the /autoconfig endpoint might help you figure out why they’re there—or not there.  
`/beans`端点产生的报告告诉你Spring应用程序上下文里都有哪些Bean，`/autoconfig`端点能告诉你为什么会有这个Bean，或者为什么没有这个Bean。

As mentioned in chapter 2, Spring Boot auto-configuration is built upon Spring conditional configuration. It provides several configuration classes with @Conditional annotations referencing conditions that decide whether or not beans should be automatically configured. The /autoconfig endpoint provides a report of all the conditions that are evaluated, grouping them by which conditions passed and which failed.  
正如第2章里说的，Spring Boot自动配置构建于Spring的条件化配置之上，它提供了众多带有`@Conditional`注解的配置类，根据条件决定是否要自动配置这些Bean。`/autoconfig`端点提供了一个报告，列出了计算过的所有条件，根据是否通过条件进行分组。

Listing 7.2 shows an excerpt from the auto-configuration report produced for the reading-list application with one passing and one failing condition.  
代码7.2是阅读列表应用程序的自动配置报告里的一个片段，里面有一个通过的条件，还有一个没通过的条件。

__Listing 7.2 An auto-configuration report for the reading-list app__  
__代码7.2 阅读列表应用程序的自动配置报告__

```
{
  "positiveMatches": {
    ...
    "DataSourceAutoConfiguration.JdbcTemplateConfiguration
                                                    #jdbcTemplate": [
      {
        "condition": "OnBeanCondition",
        "message": "@ConditionalOnMissingBean (types:
            org.springframework.jdbc.core.JdbcOperations;
            SearchStrategy: all) found no beans"
      }
    ],
    ...
  },
  "negativeMatches": {
    "ActiveMQAutoConfiguration": [
      {
        "condition": "OnClassCondition",
        "message": "required @ConditionalOnClass classes not found:
           javax.jms.ConnectionFactory,org.apache.activemq
           .ActiveMQConnectionFactory"
      }
    ],
    ...
  }
}
```

Successful conditions  
成功条件

Failed conditions  
失败条件

In the positiveMatches section, you’ll find a condition used to decide whether or not Spring Boot should auto-configure a JdbcTemplate bean. The match is named DataSourceAutoConfiguration.JdbcTemplateConfiguration#jdbcTemplate, which indicates the specific configuration class where this condition is applied. The type of condition is an OnBeanCondition, which means that the condition’s outcome is determined by the presence or absence of a bean. In this case, the message property makes it clear that the condition checks for the absence of a bean of type JdbcOperations (the interface that JbdcTemplate implements). If no such bean has already been configured, then this condition passes and a JdbcTemplate bean will be created.  
在`positiveMatches`里，你会看到一个条件，决定了Spring Boot是否自动配置`JdbcTemplate` Bean。匹配到的名字是`DataSourceAutoConfiguration.JdbcTemplateConfiguration#jdbcTemplate`，这是运用了条件的具体配置类。条件类型是`OnBeanCondition`，意味着条件的输出是由某个Bean的存在与否来决定的。在本例中，`message`属性已经清晰地表明了该条件是检查是否有`JdbcOperations`类型（`JbdcTemplate`实现了该接口）的Bean存在。如果没有配置这种Bean，条件即成立，会创建一个`JdbcTemplate` Bean。

Similarly, under negativeMatches, there’s a condition that decides whether or not to configure an ActiveMQ. This decision is an OnClassCondition, and it hinges on the presence of ActiveMQConnectionFactory in the classpath. Because ActiveMQConnectionFactory isn’t in the classpath, the condition fails and ActiveMQ will not be auto-configured.  
类似的，在`negativeMatches`里，有一个条件决定了是否要配置ActiveMQ。这是一个`OnClassCondition`，会检查Classpath里是否存在`ActiveMQConnectionFactory`。因为Classpath里没有这个类，条件不成立，不会自动配置ActiveMQ。

#### INSPECTING CONFIGURATION PROPERTIES
#### 查看配置属性

In addition to knowing how your application beans are wired together, you might also be interested in learning what environment properties are available and what configuration properties were injected on the beans.  
除了要知道应用程序的Bean是如何装配的，你可能还对能获取哪些环境属性，哪些配置属性被注入到了Bean里感兴趣。

The /env endpoint produces a list of all of the environment properties available to the application, whether they’re being used or not. This includes environment variables, JVM properties, command-line parameters, and any properties provided in an application.properties or application.yml file.  
`/env`端点会生成所有应用程序可用的环境属性的列表，无论是否被用到。其中包括环境变量、JVM属性、命令行参数，以及applicaition.properties或application.yml文件提供的属性。

The following listing shows an abridged example of what you might get from the /env endpoint.  
下面的示例代码是`/env`端点获取信息的一个片段。

__Listing 7.3 The /env endpoint reports all properties available__  
__代码7.3 `/env`端点会报告所有可用的属性__

```
{
  "applicationConfig: [classpath:/application.yml]": {
    "amazon.associate_id": "habuma-20",
    "error.whitelabel.enabled": false,
    "logging.level.root": "INFO"
  },
  "profiles": [],
  "servletContextInitParams": {},
  "systemEnvironment": {
    "BOOK_HOME": "/Users/habuma/Projects/BookProjects/walls6",
    "GRADLE_HOME": "/Users/habuma/.sdkman/gradle/current",
    "GRAILS_HOME": "/Users/habuma/.sdkman/grails/current",
    "GROOVY_HOME": "/Users/habuma/.sdkman/groovy/current",
    ...
  },
  "systemProperties": {
    "PID": "682",
    "file.encoding": "UTF-8",
    "file.encoding.pkg": "sun.io",
    "file.separator": "/",
    ...
  }
}
```

Application properties  
应用属性

Environment variables  
环境变量

JVM system properties  
JVM系统属性

Essentially, any property source that can provide properties to a Spring Boot application will be listed in the results of the /env endpoint along with the properties provided by that endpoint.  
基本上来说，任何能给Spring Boot应用程序提供属性的属性源都会列在`/env`的结果里，同时会显示具体的属性。

In listing 7.3, properties come from application configuration (application.yml), Spring profiles, servlet context initialization parameters, the system environment, and JVM system properties. (In this case, there are no profiles or servlet context initialization parameters.)  
在代码7.3中的属性来源有很多，包括应用在程序配置（application.yml）、Spring Profile、Servlet上下文初始化参数、系统环境变量和JVM系统属性。（本例中，没有Profile和Servlet上下文初始化参数。）

It’s common to use properties to carry sensitive information such as database or API passwords. To keep that kind of information from being exposed by the /env endpoint, any property named (or whose last segment is) “password”, “secret”, or “key” will be rendered as “” in the response from /env. For example, if there’s a property named “database.password”, it will be rendered in the /env response like this:  
通过属性来提供诸如数据库或API密码之类的敏感信息是种常见的用法。为了避免此类信息暴露到`/env`里，所有名字是“password”、"secret"或“key”（或者名字中最后一段是这些）的属性在`/env`里都会以“”来展现。举个例子，如果有一个属性名字是“database.password”，它的显示效果是这样的：

```
"database.password":"******"
```

The /env endpoint can also be used to request the value of a single property. Just append the property name to /env when making the request. For example, requesting /env/amazon.associate_id will yield a response of “habuma-20” (in plain text) when requested against the reading-list application.  
`/env`端点还能用来获取单个属性的值，只需要在请求时在`/env`后加上属性名即可。举例来说，对阅读列表应用程序发起`/env/amazon.associate_id`请求获得的结果是“habuma-20”（纯文本形式）。

As you’ll recall from chapter 3, these environment properties come in handy when using the @ConfigurationProperties annotation. Beans annotated with @ConfigurationProperties can have their instance properties injected with values from the environment. The /configprops endpoint produces a report of how those properties are set, whether from injection or otherwise. Listing 7.4 shows an excerpt from the configuration properties report for the reading-list application.  
回想第3章，可以很方便地通过`@ConfigurationProperties`注解来使用这些环境属性。这些环境属性会注入到带有`@ConfigurationProperties`注解的Bean的实例属性中。`/configprops`端点会生成一个报告，说明这些属性是如何进行设置的，是注入的还是其他方式。代码7.4是阅读列表应用程序的配置属性报告片段。

__Listing 7.4 A configuration properties report__  
__代码7.4 配置属性报告__

```
{
  "amazonProperties": {
    "prefix": "amazon",
    "properties": {
      "associateId": "habuma-20"
    }
  },
  ...
  "serverProperties": {
    "prefix": "server",
    "properties": {
      "address": null,
      "contextPath": null,
      "port": null,
      "servletPath": "/",
      "sessionTimeout": null,
      "ssl": null,
      "tomcat": {
        "accessLogEnabled": false,
        "accessLogPattern": null,
        "backgroundProcessorDelay": 30,
        "basedir": null,
        "compressableMimeTypes": "text/html,text/xml,text/plain",
        "compression": "off",
        "maxHttpHeaderSize": 0,
        "maxThreads": 0,
        "portHeader": null,
        "protocolHeader": null,
        "remoteIpHeader": null,
        "uriEncoding": null
      },
      ...
    }
  },
  ...
}
```

Amazon configuration  
Amazon配置

Server configuration  
服务器配置

The first item in this excerpt is the amazonProperties bean we created in chapter 3. This report tells us that it’s annotated with @ConfigurationProperties to have a prefix of “amazon”. And it shows that the associateId property is set to “habuma-20”. This is because in application.yml, we set the amazon.associateId property to “habuma-20”.  
片段中的第一个内容是我们在第3章里创建的`amazonProperties` Bean。报告显示它添加了`@ConfigurationProperties`注解，前缀为“amazon”。`associateId`属性被设置为“habuma-20”。这是因为在application.yml里，我们把`amazon.associateId`属性设置成了“habuma-20”。

You can also see an entry for serverProperties—it has a prefix of “server” and several properties that we can work with. Here they all have default values, but you can change any of them by setting a property prefixed with “server”. For example, you could change the port that the server listens on by setting the server.port property.  
你还会看到一个`serverProperties`条目——它的前缀是“server”，还有一些属性。它们都有默认值，你也可以通过设置“server”前缀的属性来改变这些值。举例来说，你可以通过设置`server.port`属性来修改服务器监听的端口。

Aside from giving insight into how configuration properties are set in the running application, this report is also useful as a quick reference showing all of the properties that you could set. For example, if you weren’t sure how to set the maximum number of threads in the embedded Tomcat server, a quick look at the configuration properties report would give you a clue that server.tomcat.maxThreads is the property you’re looking to set.  
除了能了解运行中应用程序的配置属性是如何设置的，这个报告也能作为一个快速参考指南，告诉你有哪些属性可以设置。例如，如果你不清楚怎么设置嵌入式Tomcat服务器的最大线程数，可以看一下配置属性报告，里面会有一条`server.tomcat.maxThreads`，这就是你要找的属性。

#### PRODUCING ENDPOINT-TO-CONTROLLER MAP
#### 生成端点到控制器的映射

When an application is relatively small, it’s usually easy to know how all of its controllers are mapped to endpoints. But once the web interface exceeds more than a handful of controllers and request-handling methods, it might be helpful to have a list of all of the endpoints exposed by the application.  
在应用程序相对较小的时候，很容易就能搞清楚控制器都映射到了哪些端点上。一旦Web界面的控制器和请求处理方法数量多了，最好就能有一个列表，罗列出应用程序发布的全部端点。

The /mappings endpoint provides such a list. Listing 7.5 shows an excerpt of the mappings report from the reading-list application.  
`/mappings`端点就提供了这么一个列表。代码7.5是阅读列表应用程序的映射报告片段。

__Listing 7.5 The controller/endpoint mappings for the reading-list app__  
__代码7.5 阅读列表应用程序的控制器/端点映射__

```
{
  ...

  "{[/],methods=[GET],params=[],headers=[],consumes=[],produces=[],
                                                       custom=[]}": {
    "bean": "requestMappingHandlerMapping",
    "method": "public java.lang.String readinglist.ReadingListController.
              readersBooks(readinglist.Reader,org.springframework.ui.Model)"
  },
  "{[/],methods=[POST],params=[],headers=[],consumes=[],produces=[],
                                                        custom=[]}": {
    "bean": "requestMappingHandlerMapping",
    "method": "public java.lang.String readinglist.ReadingListController
                            .addToReadingList(readinglist.Reader,readinglist.
     Book)"
  },
  "{[/autoconfig],methods=[GET],params=[],headers=[],consumes=[]
                                          ,produces=[],custom=[]}": {
    "bean": "endpointHandlerMapping",
    "method": "public java.lang.Object org.springframework.boot
                   .actuate.endpoint.mvc.EndpointMvcAdapter.invoke()"
  },
  ...
}
```

ReadingListController mappings
`ReadingListController`映射

Auto-configuration report mapping  
自动配置报告的映射

Here we see a handful of endpoint mappings. The key for each mapping is a string containing what appears to be the attributes of Spring MVC’s @RequestMapping annotation. Indeed, this string gives you a good idea of how the controller is mapped, even if you haven’t seen the source code. The value of each mapping has two properties: bean and method. The bean property identifies the name of the Spring bean that the mapping comes from. The method property gives the fully qualified method signature of the method for which the mapping is being reported.  
这里我们可以看到一堆端点的映射。每个映射的键是一个字符串，其内容就是Spring MVC的`@RequestMapping`注解上设置的属性。实际上，这个字符串能让你清晰地了解到控制器是如何映射的，哪怕不看源代码都能知道。每个映射的值都有两个属性：`bean`和`method`。`bean`属性标识了Spring Bean的名字，映射源自这个Bean。`method`属性是映射对应方法的全限定方法签名。

The first two mappings are for the request-handing methods in our application’s ReadingListController. The first shows that an HTTP GET request for the root path (“/”) will be handled by the readersBooks() method. The second shows that a POST request is mapped to the addToReadingList() method.  
头两个映射是应用程序的`ReadingListController`的请求处理方法。第一个是处理根路径（“/”）的HTTP GET请求的，该请求由`readersBooks()`方法来处理。第二个POST请求映射到了`addToReadingList()`方法上。

The next mapping is for an Actuator-provided endpoint. An HTTP GET request for the /autoconfig endpoint will be handled by the invoke() method of Spring Boot’s EndpointMvcAdapter class. There are, of course, many other Actuator endpoints that aren’t shown in listing 7.5, but those were omitted from the listing for brevity’s sake.  
接下来的映射是Actuator提供的端点。`/autoconfig`端点的HTTP GET请求由Spring Boot的`EndpointMvcAdapter`类的`invoke()`方法来处理。当然，还有很多其他Actuator的端点没有列在代码7.5里，这种省略完全是为了简化代码示例。

The Actuator’s configuration endpoints are great for seeing how your application is configured. But it’s also interesting and useful to see what’s actually happening within your application while it’s running. The metrics endpoints help give a snapshot into an application’s runtime internals.  
Actuator的配置端点能很方便地让你了解应用程序是如何配置的。能看到应用程序在运行时究竟发生了什么同样也是一件很有趣的事，度量端点能展示应用程序运行时内部状况的快照。

### 7.1.2 Tapping runtime metrics
### 7.1.2 运行时度量

When you go to the doctor for a physical exam, the doctor performs a battery of tests to see how your body is performing. Some of them, such as determining your blood type, are important but will not change over time. These kinds of tests give the doctor insight into how your body is configured. Other tests give the doctor a snapshot into how your body is performing during the visit. Your heart rate, blood pressure, and cholesterol level are useful in helping the doctor evaluate your health. These metrics are temporal and likely to change over time, but they’re still helpful runtime metrics.  
当你到医生那里做体检时，医生会做一系列检查来了解你的身体状况。其中有一些很重要，但却永远不会变，比如确定血型。这类测试让医生了解你身体的构成。其他测试让医生掌握你检查时的身体状况。你的心律、血压和胆固醇水平对医生评估你的健康情况很有帮助。这些指标都是临时的，很可能随着时间而发生变化，但它们同样是很有帮助的运行时指标。

Similarly, taking a snapshot of the runtime metrics is helpful in evaluating the health of an application. The Actuator offers a handful of endpoints that enable you to perform a quick checkup on your application while it’s running. Let’s take a look at them, starting with the /metrics endpoint.  
类似的，对运行时度量情况做一个快照，这对评估应用程序的健康情况很有帮助。Actuator提供了一系列端点让你能在运行时快速检查应用程序。让我们来了解一下这些端点，从`/metrics`开始。

#### VIEWING APPLICATION METRICS  
#### 查看应用程序的度量值

There are a lot of interesting and useful bits of information about any running application. Knowing the application’s memory circumstances (available vs. free), for instance, might help you decide if you need to give the JVM more or less memory to work with. For a web application, it can be helpful knowing at a glance, without scouring web server log files, if there are any requests that are failing or taking too long to serve.  
关于运行中的应用程序，有很多有趣而且有用的信息。举个例子，了解应用程序的内存情况（可用 vs. 空闲）有助于帮你决定多给JVM一些内存，还是少分配一些。对Web应用程序而言，不用查看Web服务器日志，如果有请求失败或者是耗时太长，就可以大概知道内存的大概情况了。

The /metrics endpoint provides a snapshot of various counters and gauges in a running application. The following listing shows a sample of what the /metrics endpoint might give you.  
运行中的应用程序有诸多计数器和度量器，`/metrics`端点提供了这些东西的快照。下面的代码是`/metrics`端点输出内容的示例。

__Listing 7.6 The metrics endpoint provides several useful pieces of runtime data__  
__代码7.6 `/metrics`端点提供了很多有用的运行时数据__

```
{
  mem: 198144,
  mem.free: 144029,
  processors: 8,
  uptime: 1887794,
  instance.uptime: 1871237,
  systemload.average: 1.33251953125,
  heap.committed: 198144,
  heap.init: 131072,
  heap.used: 54114,
  heap: 1864192,
  threads.peak: 21,
  threads.daemon: 19,
  threads: 21,
  classes: 9749,
  classes.loaded: 9749,
  classes.unloaded: 0,
  gc.ps_scavenge.count: 22,
  gc.ps_scavenge.time: 122,
  gc.ps_marksweep.count: 2,
  gc.ps_marksweep.time: 156,
  httpsessions.max: -1,
  httpsessions.active: 1,
  datasource.primary.active: 0,
  datasource.primary.usage: 0,
  counter.status.200.beans: 1,
  counter.status.200.env: 1,
  counter.status.200.login: 3,
  counter.status.200.metrics: 2,
  counter.status.200.root: 6,
  counter.status.200.star-star: 9,
  counter.status.302.login: 3,
  counter.status.302.logout: 1,
  counter.status.302.root: 5,
  gauge.response.beans: 169,
  gauge.response.env: 165,
  gauge.response.login: 3,
  gauge.response.logout: 0,
  gauge.response.metrics: 2,
  gauge.response.root: 11,
  gauge.response.star-star: 2
}
```

As you can see, a lot of information is provided by the /metrics endpoint. Rather than examine these metrics line by line, which would be tedious, table 7.2 groups them into categories by the type of information they offer.  
如你所见，`/metrics`端点了很多信息，逐行查看这些度量值太麻烦了，表7.2根据所提供信息的类型对它们做了个分类。

__Table 7.2 Gauges and counters reported by the /metrics endpoint__  
__表7.2 `/metrics`端点报告的度量值和计数器__
