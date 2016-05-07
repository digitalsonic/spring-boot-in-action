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

| Category | Prefix            | What it reports                                   |
|----------|-------------------|---------------------------------------------------|
| Garbage collector | gc.* | The count of garbage collections that have occurred and the elapsed garbage collection time for both the mark-sweep and scavenge garbage collectors (from java.lang .management.GarbageCollectorMXBean) |
| Memory | mem.* | The amount of memory allotted to the application and the amount of memory that is free (from java.lang.Runtime) |
| Heap | heap.* | The current memory usage (from java.lang .management.MemoryUsage) |
| Class loader | classes.* | The number of classes that have been loaded and unloaded by the JVM class loader (from java.lang.management .ClassLoadingMXBean) |
| System | processors  uptime  instance.uptime  systemload.average | System information such as the number of processors (from java.lang.Runtime), uptime (from java.lang .management.RuntimeMXBean), and average system load (from java.lang.management .OperatingSystemMXBean) |
| Thread pool | threads.* | The number of threads, daemon threads, and the peak count of threads since the JVM started (from java.lang .management.ThreadMXBean) |
| Data source | datasource.* | The number of data source connections (from the data source’s metadata and only available if there are one or more DataSource beans in the Spring application context) |
| Tomcat sessions | httpsessions.* | The active and maximum number of sessions in Tomcat (from the embedded Tomcat bean and only available if the application is served via an embedded Tomcat server) |
| HTTP | counter.status.*  gauge.response.* | Various gauges and counters for HTTP requests that the application has served |

| 分类      | 前缀              | 报告内容                                            |
|----------|-------------------|---------------------------------------------------|
| 垃圾收集器 | `gc.*` | 已经发生过的垃圾收集次数，以及垃圾收集所耗费的时间，适用于标记-清理垃圾收集器和并行垃圾收集器（数据源自`java.lang.management.GarbageCollectorMXBean`） |
| 内存 | `mem.*` | 分配给应用程序的内存数量和空闲的内存数量（数据源自`java.lang.Runtime`） |
| 堆 | `heap.*` | 当前内存用量（数据源自`java.lang.management.MemoryUsage`） |
| 类加载器 | `classes.*` | JVM类加载器加载与卸载的类的数量（数据源自`java.lang.management.ClassLoadingMXBean`） |
| 系统 | `processors`  `uptime`  `instance.uptime`  `systemload.average` | 系统信息，例如处理器数量（数据源自`java.lang.Runtime`）、运行时间（数据源自`java.lang.management.RuntimeMXBean`）、平均负载（数据源自`java.lang.management.OperatingSystemMXBean`） |
| 线程池 | `threads.*` | 线程、守护线程的数量，以及JVM启动后的线程数量峰值（数据源自`java.lang .management.ThreadMXBean`） |
| 数据源 | `datasource.*` | 数据源连接的数量（数据源自数据源的元数据，仅当Spring应用程序上下文里存在`DataSource` Bean的时候才会有这个信息） |
| Tomcat会话 | `httpsessions.*` | Tomcat的活跃会话数和最大会话数（数据源自嵌入式Tomcat的Bean，仅在使用嵌入式Tomcat服务器运行应用程序时才有这个信息） |
| HTTP | `counter.status.*`  `gauge.response.*` | 多种应用程序服务的HTTP请求的度量值与计数器 |

Notice that some of these metrics, such as the data source and Tomcat session metrics, are only available if the necessary components are in play in the running application. You can also register your own custom application metrics, as you’ll see in section 7.4.3.  
请注意，这里的一些度量值，比如数据源和Tomcat会话，仅在应用程序中存在特定组件时才有数据。你还可以注册自己的度量信息，7.4.3节里会提到的。

The HTTP counters and gauges demand a bit more explanation. The number following the counter.status prefix is the HTTP status code. What follows that is the path requested. For instance, the metric named counter.status.200.metrics indicates the number of times that the /metrics endpoint was served with an HTTP status of 200 (OK).  
HTTP的计数器和度量值需要做一点说明。`counter.status`后的值是HTTP状态码，随后是所请求的路径。举个例子，`counter.status.200.metrics`表明了`/metrics`端点返回200（OK）状态码的次数。

The HTTP gauges are similarly structured but report a different kind of metrics. They’re all prefixed with gauge.response, indicating that they are gauges for HTTP responses. Following that prefix is the path that the gauge refers to. The value of the metric indicates the time in milliseconds that it took to serve that path the most recent time it was served. For instance, the gauge.response.beans metric in table 7.6 indicates that it took 169 milliseconds to serve that request the last time it was served.  
HTTP的度量信息在结构上也差不多，但是却在报告另一类信息。它们全部用`gauge.response`开头，表明这是HTTP响应的度量信息。前缀后是对应的路径，度量值是以毫秒为单位的时间，反映了最近处理该路径请求的耗时。举个例子，代码7.6里的`gauge.response.beans`说明上一次请求耗时169毫秒。

You’ll notice that there are a few special cases for the counter and gauge paths. The root path refers to the root path or /. And star-star is a catchall that refers to any path that Spring determines is a static resource, including images, JavaScript, and stylesheets. It also includes any resource that can’t be found, which is why you’ll often see a counter.status.404.star-star metric indicating the count of requests that were met with HTTP 404 (NOT FOUND) status.  
这里还有几个特殊的值需要注意。`root`路径指向的是根路径或`/`。`star-star`代表了那些Spring认为是静态资源的路径，包括图片、JavaScript和样式表，其中还包含了那些找不到的资源，这就是为什么你经常会看到`counter.status.404.star-star`，这是返回了HTTP 404（NOT FOUND）状态的请求数。

Whereas the /metrics endpoint fetches a full set of all available metrics, you may only be interested in a single metric. To fetch only one metric value, append the metric’s key to the URL path when making the request. For example, to fetch only the amount of free memory, perform a GET request for /metrics/mem.free:  
`/metrics`端点会返回所有的可用度量值，但你也可能只对某个值感兴趣。要获取单个值，可以在URL后加上对应的键名。例如，要查看空闲内存大小，可以向`/metrics/mem.free`发一个GET请求：

```
$ curl localhost:8080/metrics/mem.free
144029
```

It may be useful to know that even though the result from /metrics/{name} appears to be plain text, the Content-Type header in the response is set to “application/json;charset=UTF-8”. Therefore, it can be processed as JSON if you need to do so.  
虽然响应里的`Content-Type`头设置为“application/json;charset=UTF-8”，但实际`/metrics/{name}`的结果是文本格式的。因此，如果需要的话，你也可以把它视为JSON来处理。

#### TRACING WEB REQUESTS
#### 追踪Web请求

Although the /metrics endpoint gives you some basic counters and timers for web requests, those metrics lack any details. Sometimes it can be helpful, especially when debugging, to know more about the requests that were handled. That’s where the /trace endpoint can be handy.  
尽管`/metrics`端点提供了一些针对Web请求的基本计数器和计时器，但那些度量值缺少详细信息。知道所处理的请求的更多信息是很有帮助的，尤其是在调试时，所以就有了`/trace`这个端点。

The /trace endpoint reports details of all web requests, including details such as the request method, path, timestamp, and request and response headers. Listing 7.7 shows an excerpt of the /trace endpoint’s output containing a single request trace entry.  
`/trace`端点能报告所有Web请求的详细信息，包含请求方法、路径、时间戳以及请求和响应的头信息。代码7.7是`/trace`输出的一个片段，其中包含了一整个请求跟踪项。

__Listing 7.7 The /trace endpoint records web request details__  
__代码7.7 `/trace`端点会记录下Web请求的细节__

```
[
  ...
  {
    "timestamp": 1426378239775,
    "info": {
      "method": "GET",
      "path": "/metrics",
      "headers": {
        "request": {
          "accept": "*/*",
          "host": "localhost:8080",
          "user-agent": "curl/7.37.1"
        },
        "response": {
          "X-Content-Type-Options": "nosniff",
          "X-XSS-Protection": "1; mode=block",
          "Cache-Control":
                    "no-cache, no-store, max-age=0, must-revalidate",
          "Pragma": "no-cache",
          "Expires": "0",
          "X-Frame-Options": "DENY",
          "X-Application-Context": "application",
          "Content-Type": "application/json;charset=UTF-8",
          "Transfer-Encoding": "chunked",
          "Date": "Sun, 15 Mar 2015 00:10:39 GMT",
          "status": "200"
        }
      }
    }
  }
]
```

As indicated by the method and path properties, you can see that this trace entry is for a /metrics request. The timestamp property (as well as the Date header in the response) tells you when the request was handled. The headers property carries header details for both the request and the response.  
正如`method`和`path`属性所示，你可以看到这个跟踪项是一个针对`/metrics`的请求。`timestamp`属性（以及响应中的`Date`头）告诉了你请求的处理时间。`headers`属性的内容是请求和响应中所携带的头信息。

Although listing 7.7 only shows a single trace entry, the /trace endpoint will report trace details for the 100 most recent requests, including requests for the /trace endpoint itself. It maintains the trace data in an in-memory trace repository. Later, in section 7.4.4, you’ll see how to create a custom trace repository implementation for a more permanent tracing of requests.  
虽然代码7.7里只显示了一条跟踪项，但`/trace`端点实际能显示最近100个请求的信息，包含对`/trace`自己的请求，它在内存里维护了一个跟踪库。稍后在7.4.4节里，你会看到如何创建一个自定义的跟踪库实现，以便持久化请求的跟踪信息。

#### DUMPING THREAD ACTIVITY
#### 导出线程活动

In addition to request tracing, thread activity can also be useful in determining what’s going on in a running application. The /dump endpoint produces a snapshot of current thread activity.  
在确认应用程序运行情况时，除了跟踪请求，了解线程活动也会很有帮助。`/dump`端点会生成当前线程活动的快照。

__Listing 7.8 The /dump endpoint provides a snapshot of an application’s threads__  
__代码7.8 `/dump`端点提供了应用程序线程的快照__

```
[
  {
    "threadName": "container-0",
    "threadId": 19,
    "blockedTime": -1,
    "blockedCount": 0,
    "waitedTime": -1,
    "waitedCount": 64,
    "lockName": null,
    "lockOwnerId": -1,
    "lockOwnerName": null,
    "inNative": false,
    "suspended": false,
    "threadState": "TIMED_WAITING",
    "stackTrace": [
      {
        "className": "java.lang.Thread",
        "fileName": "Thread.java",
        "lineNumber": -2,
        "methodName": "sleep",
        "nativeMethod": true
      },
      {
        "nativeMethod": false
      },
      {
        "className": "org.springframework.boot.context.embedded.
                            tomcat.TomcatEmbeddedServletContainer$1",
        "fileName": "TomcatEmbeddedServletContainer.java",
        "lineNumber": 139,
        "methodName": "run",
        "nativeMethod": false
      }
    ],
    "lockedMonitors": [],
    "lockedSynchronizers": [],
    "lockInfo": null
  },
  ...
]
```

The complete thread dump report includes every thread in the running application. To save space, listing 7.8 shows an abridged entry for a single thread. As you can see, it includes details regarding the blocking and locking status of the thread, among other thread specifics. There’s also a stack trace that, in this case, indicates the thread is a Tomcat container thread.  
完整的线程导出报告里会包含应用程序的每个线程，为了节省空间，代码7.8里只放了一个线程的内容片段。如你所见，其中包含了很多线程的特定信息，还有线程相关的阻塞和锁状态。本例中，还有一个跟踪栈（stack trace），表明这是一个Tomcat容器线程。

#### MONITORING APPLICATION HEALTH
#### 监控应用程序健康情况

If you’re ever wondering if your application is up and running or not, you can easily find out by requesting the /health endpoint. In the simplest case, the /health endpoint reports a simple JSON structure like this:  
如果你想知道自己的应用程序是否在运行，可以直接访问`/health`端点。在最简单的情况下，该端点会显示一个简单的JSON，内容如下：

```
{"status":"UP"}
```

The status property reports that the application is up. Of course it is. It doesn’t really matter what the response is; any response at all is an indication that the application is running. But the /health endpoint has more information than a simple “UP” status.  
`status`属性显示了应用程序在运行中。当然，它的确在运行，此处的响应无关紧要，任何输出都说明这个应用程序在运行。但`/health`端点可以输出的信息远远不止简单的“UP”状态。

Some of the information offered by the /health endpoint can be sensitive, so unauthenticated requests are only given the simple health status response. If the request is authenticated (for example, if you’re logged in), more health information is exposed. Here’s some sample health information reported for the reading-list application:  
`/health`端点输出的某些信息可能涉及内容，因此未经授权的请求只能提供简单的健康状态。如果是经过身份验证的（比如你已经登录了），则可以提供更多信息。下面是一些阅读列表应用程序的健康信息示例：

```
{
  "status":"UP",
  "diskSpace": {
    "status":"UP",
    "free":377423302656,
    "threshold":10485760
  },
  "db":{
    "status":"UP",
    "database":"H2",
    "hello":1
  }
}
```

Along with the basic health status, you’re also given information regarding the amount of available disk space and the status of the database that the application is using.  
除了基本的健康状态，还可以看到可用的磁盘空间以及应用程序正在使用的数据库状态。

All of the information reported by the /health endpoints is provided by one or more health indicators, including those listed in table 7.3, that come with Spring Boot.  
`/health`端点所提供的所有信息都是由一个或多个健康指示器提供的，表7.3列出了Spring Boot自带的健康指示器。

__Table 7.3 Spring Boot’s out-of-the-box health indicators__  
__表7.3 Spring Boot自带的健康指示器__

| Health indicator | Key | Reports |
|------------------|-----|---------|
| `ApplicationHealthIndicator` | none | Always “UP” |
| `DataSourceHealthIndicator` | db | “UP” and database type if the database can be contacted; “DOWN” status otherwise |
| `DiskSpaceHealthIndicator` | diskSpace | “UP” and available disk space, and “UP” if avail- able space is above a threshold; “DOWN” if there isn’t enough disk space |
| `JmsHealthIndicator` | jms | “UP” and JMS provider name if the message bro- ker can be contacted; “DOWN” otherwise |
| `MailHealthIndicator` | mail | “UP” and the mail server host and port if the mail server can be contacted; “DOWN” otherwise |
| `MongoHealthIndicator` | mongo | “UP” and the MongoDB server version; “DOWN” otherwise |
| `RabbitHealthIndicator` | rabbit | “UP” and the RabbitMQ broker version; “DOWN” otherwise |
| `RedisHealthIndicator` | redis | “UP” and the Redis server version; “DOWN” otherwise |
| `SolrHealthIndicator` | solr | “UP” if the Solr server can be contacted; “DOWN” otherwise |

| 健康指示器 | 键名 | 报告内容 |
|----------|-----|---------|
| ApplicationHealthIndicator | `status` | 永远是“UP” |
| DataSourceHealthIndicator | `db` | 如果数据库能连上，则内容是“UP”和数据库类型；否则是“DOWN” |
| DiskSpaceHealthIndicator | `diskSpace` | 如果可用空间大于阈值，则内容为“UP”和可用磁盘空间；如果空间不足则为“DOWN” |
| JmsHealthIndicator | `jms` | 如果能连上消息代理，则内容为“UP”和JMS提供方的名称；否则为“DOWN” |
| MailHealthIndicator | `mail` | 如果能连上邮件服务器，则内容为“UP”和邮件服务器主机和端口；否则为“DOWN” |
| MongoHealthIndicator | `mongo` | 如果能连上MongoDB服务器，则内容为“UP”和MongoDB服务器版本；否则为“DOWN” |
| RabbitHealthIndicator | `rabbit` | 如果能连上RabbitMQ服务器，则内容为“UP”和版本号；否则为“DOWN” |
| RedisHealthIndicator | `redis` | 如果能连上服务器，则内容为“UP”和Redis服务器版本；否则为“DOWN” |
| SolrHealthIndicator | `solr` | 如果能连上Solr服务器，则内容为“UP”；否则为“DOWN” |

These health indicators will be automatically configured as needed. For example, DataSourceHealthIndicator will be automatically configured if javax.sql.DataSource is available in the classpath. ApplicationHealthIndicator and DiskSpaceHealthIndicator will always be configured.  
这些健康指示器会按需自动配置。举例来说，如果Classpath里有`javax.sql.DataSource`则会自动配置`DataSourceHealthIndicator`。`ApplicationHealthIndicator`和`DiskSpaceHealthIndicator`则一直会被配置。

In addition to these out-of-the-box health indicators, you’ll see how to create custom health indicators in section 7.4.5.  
除了这些自带的健康指示器，在7.4.5节里你还会看到如何创建自定义健康指示器。

### 7.1.3 Shutting down the application
### 7.1.3 关闭应用程序

Suppose you need to kill your running application. In a microservice architecture, for instance, you might have multiple instances of a microservice application running in the cloud. If one of those instances starts misbehaving, you might decide to shut it down and let the cloud provider restart the failed application for you. In that scenario, the Actuator’s /shutdown endpoint will prove useful.  
假设你要关闭运行中的应用程序。比方说，在微服务架构中，你有多个微服务应用的实例运行在云上，其中某个实例有问题了，你决定关闭该实例并让云服务提供商为你重启这个有问题的应用程序。在这个场景中，Actuator的`/shutdown`端点就很有用了。

In order to shut down your application, you can send a POST request to /shutdown. For example, you can shut down your application using the curl command-line tool like this:  
为了关闭应用程序，你要往`/shutdown`发送一个POST请求。例如，可以用命令行工具curl来关闭应用程序：

```
$ curl -X POST http://localhost:8080/shutdown
```

Obviously, the ability to shut down a running application is a dangerous thing, so it’s disabled by default. Unless you’ve explicitly enabled it, you’ll get the following response from the POST request:  
很显然，关闭运行中的应用程序是件危险的事情，因此这个端点默认是关闭的。如果没有显式地开启这个功能，那么POST请求的结果是这样的：

```
{"message":"This endpoint is disabled"}
```

To enable the /shutdown endpoint, configure the endpoints.shutdown.enabled property to true. For example, add the following lines to application.yml to enable the /shutdown endpoint:  
要开启该端点，可以将`endpoints.shutdown.enabled`设置为`true`。举例来说，可以把如下内容加入application.yml里，藉此开启`/shutdown`端点：

```
endpoints:
  shutdown:
    enabled: true
```

Once the /shutdown endpoint is enabled, you want to make sure that not just anybody can kill your application. You should secure the /shutdown endpoint, requiring that only authorized users are allowed to bring the application down. You’ll see how to secure Actuator endpoints in section 7.5.  
打开`/shutdown`端点后，你不会希望谁都能关闭应用程序的，这时就应该要保护`/shutdown`端点，只有经过授权的用户能关闭应用程序。在7.5节里你将看到如何保护Actuator端点。

### 7.1.4 Fetching application information
### 7.1.4 获取应用信息

Spring Boot’s Actuator has one more endpoint you might find useful. The /info endpoint reports any information about your application that you might want to expose to callers. The default response to a GET request to /info looks like this:  
Spring Boot Actuator还有一个有用的端点，`/info`端点能展示各种你希望发布的应用信息。针对该端点的GET请求的默认响应是这样的：

```
{}
```

Obviously, an empty JSON object isn’t very useful. But you can add any information to the /info endpoint’s response by simply configuring properties prefixed with info. For example, suppose you want to provide a contact email in the /info endpoint response. You could set a property named info.contactEmail like this in application.yml:  
很显然，一个空的JSON对象没什么用。但你可以通过配置带有`info`前缀的属性向`/info`端点的响应中添加内容。例如，你希望在响应中添加联系邮箱。可以在application.yml里设置名为`info.contactEmail`的属性：

```
info:
  contactEmail: support@myreadinglist.com
```

Now if you request the /info endpoint, you’ll get the following response:  
现在再访问`/info`端点，就能得到如下响应：

```
{
  "contactEmail":"support@myreadinglist.com"
}
```

Properties in the /info response can also be nested. For example, suppose that you want to provide both a support email and a support phone number. In application.yml, you might configure the following properties:  
这里的属性也能是嵌套的。例如，假设你希望提供联系邮箱和电话。在application.yml里可以配置如下属性：

```
info:
  contact:
    email: support@myreadinglist.com
    phone: 1-888-555-1971
```

The JSON returned from the /info endpoint will include a contact property that itself has email and phone properties:  
`/info`端点返回的JSON会包含一个`contact`属性，其中有`email`和`phone`属性：

```
{
  "contact":{
    "email":"support@myreadinglist.com",
    "phone":"1-888-555-1971"
  }
}
```

Adding properties to the /info endpoint is just one of many ways you can customize Actuator behavior. Later in section 7.4, we’ll look at other ways that you can configure and extend the Actuator. But first, let’s see how to secure the Actuator’s endpoints.  
向`/info`端点添加属性只是众多定制Actuator行为的方式里的一种。稍后在7.4节里，我们还会看到其他配置与扩展Actuator的方式。但现在，先让我们来看看如何保护Actuator的端点。

## 7.2 Connecting to the Actuator remote shell
## 7.2 连接Actuator的远程Shell

You’ve seen how the Actuator provides some very useful information over REST endpoints. An optional way to dig into the internals of a running application is by way of a remote shell. Spring Boot integrates with CRaSH, a shell that can be embedded into any Java application. Spring Boot also extends CRaSH with a handful of Spring Boot-specific commands that offer much of the same functionality as the Actuator’s endpoints.  
Actuator通过REST端点提供了不少非常有用的信息。另一个深入运行中应用程序内部的方式是使用远程Shell。Spring Boot集成了CRaSH，一种能嵌入任意Java应用程序的Shell。Spring Boot还扩展了CRaSH，添加了不少Spring Boot特有的命令，提供了与Actuator端点类似的功能。

In order to use the remote shell, you’ll need to add the remote shell starter as a dependency. The Gradle dependency you’ll need looks like this:  
要使用远程Shell，只需加入远程Shell的起步依赖即可。

```
compile("org.springframework.boot:spring-boot-starter-remote-shell")
```

If you’re building your project with Maven, you’ll need the following dependency in your pom.xml file:  
如果你是用Maven来构建项目的，需要在pom.xml文件里添加如下依赖：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-remote-shell</artifactId>
</dependency>
```

And if you’re developing an application to run with the Spring Boot CLI, the following @Grab is what you’ll need:  
如果你要用Spring Boot CLI来运行你所开发的应用程序，则需要如下的`@Grab`注解：

```
@Grab("spring-boot-starter-remote-shell")
```

With the remote shell added as a dependency, you can now build and run the application. As it’s starting up, watch for the password to be written to the log in a line that looks something like this:  
添加了远程Shell依赖后，就可以构建并运行应用程序了。在启动的时候，可以看到日志里有一行密码，大概是这样的：

```
Using default security password: efe30c70-5bf0-43b1-9d50-c7a02dda7d79
```

The username that goes with that password is “user”. The password itself is randomly generated and will be different each time you run the application.  
与这个密码搭配使用的用户名是“user”，密码本身是随机生成的，每次运行应用程序时都会有所变化。

Now you can use an SSH utility to connect to the shell, which is listening for connections on port 2000. If you use the UNIX ssh command to connect to the shell, it might look something like this:  
现在你可以通过SSH工具连接Shell了，它监听的端口号是2000。如果你用的是UNIX的ssh命令，看起来大概是这样的：

```
~% ssh user@localhost -p 2000
Password authentication
Password:
. ____ _ ____ /\\/___'_____(_)___ ___\\\\ ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
\\/ ___)||_)|||||||(_|| )))) ' |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::  (v1.3.0.RELEASE) on habuma.local
>
```

Great! You’re connected to the shell. Now what?  
太棒了！你已经连上Shell了。现在该干什么？

The remote shell offers almost two dozen commands that you can execute within the context of the running application. Most of those commands come out of the box with CRaSH, but Spring Boot adds a handful of commands. These Spring Boot-specific commands are listed in table 7.4.  
远程Shell提供了二十四个可以在应用程序上下文中执行的命令，其中的大部分命令都是CRaSH自带的，但Spring Boot也添加了一些。表7.4列出了这些Spring Boot特有的命令。

__Table 7.4 CRaSH shell commands provided by Spring Boot__  
__表7.4 Spring Boot提供的CRaSH Shell命令__

| Command | Description |
|---------|-------------|
| autoconfig | Produces an auto-configuration explanation report. Similar to the /autoconfig end- point, except that the results are plain text instead of JSON. |
| beans | Displays the beans in the Spring application context. Similar to the /beans endpoint. |
| endpoint | Invokes an Actuator endpoint. |
| metrics | Displays Spring Boot metrics. Similar to the /metrics endpoint, except presented as a live list of metrics that’s updated as the values change. |

| 命令 | 描述 |
|-----|-----|
| `autoconfig` | 生成自动配置说明报告，和`/autoconfig`端点输出的内容类似，只是把JSON换成了纯文本。 |
| `beans` | 列出Spring应用程序上下文里的Bean。与`/beans`端点输出的内容类似。 |
| `endpoint` | 调用Actuator端点。 |
| `metrics` | 显示Spring Boot的度量信息。与`/metrics`端点类似，但显示的是实时更新的数据。 |

Let’s take a look at how to use each of the shell commands added by Spring Boot.  
让我们看看如何使用这些Spring Boot添加的Shell命令。

### 7.2.1 Viewing the autoconfig report
### 7.2.1 查看`autoconfig`报告

The autoconfig command produces a report that’s very similar to the report produced by the Actuator’s /autoconfig endpoint. Figure 7.1 shows an abridged screenshot of the output produced by the autoconfig command.  
`autoconfig`命令生成了一个与Actuator `/autoconfig`端点类似的报告。图7.1是`autoconfig`命令输出的内容截屏片段。

As you can see, the results are split into two groups—positive matches and negative matches—just like the results from the /autoconfig endpoint. In fact, the only significant difference is that the autoconfig command produces a textual report whereas the /autoconfig endpoint produces JSON output. Otherwise, they are the same.  
如你所见，结果被分成两组——匹配和不匹配，就和`/autoconfig`端点的结果一样。实际上，唯一的显著区别就是`autoconfig`命令输出的是文本，而`/autoconfig`端点输出的是JSON，其他都是一样的。

![图7.1](../Figures/figure-7.1.png)

__Figure 7.1 Output of autoconfig command__
__图7.1 `autoconfig`命令的输出__

We’re not going to dwell on any of the shell commands provided natively by CRaSH, but you might want to consider piping the results of the autoconfig command to CRaSH’s less command:  
我不打算去讲那些CRaSH自己提供的Shell命令，但你可能会想把`autoconfig`命令的输出和CRaSH的`less`命令用管道串起来：

```
> autoconfig | less
```

The less command is much like the same-named command in Unix shells; it enables you to page back and forth through a file. The autoconfig output is lengthy, but piping it to less will make it easier to read and navigate.  
`less`命令和Unix Shell里的同名命令很像；能让你穿梭于文件中。`autoconfig`的输出很长，但通过`less`命令会让它更容易查阅。

### 7.2.2 Listing application beans
### 7.2.2 列出应用程序的Bean

The output from the autoconfig shell command and the /autoconfig endpoint were similar but different. In contrast, you’ll find that the results from the beans command are exactly the same as those from the /beans endpoint, as the screenshot in figure 7.2 shows.  
`autoconfig`命令的输出和`/autoconfig`端点的输出很类似，但也有不同。对比之下，你会发现`beans`命令的输出则和`/beans`端点的输出一样，截屏如图7.2所示。

![图7.2](../Figures/figure-7.2.png)

__Figure 7.2 Output of beans command__  
__图7.2 `beans`命令的输出__

Just like the /beans endpoint, the beans command produces a list of all beans in the
Spring application context, along with any dependency beans, in JSON format.  
和`/beans`端点一样，`beans`命令会以JSON格式列出Spring应用程序上下文里所有的Bean，包括所依赖的Bean。

### 7.2.3 Watching application metrics
### 7.2.3 查看应用程序的度量信息

The metrics shell command produces the same information as the Actuator /metrics endpoint. But unlike the /metrics endpoint, which produces a snapshot of the current metrics in JSON format, the metrics command takes over the shell and displays its results in a live dashboard. Figure 7.3 shows what the metrics dashboard looks like.  
`metrics`命令会输出与Actuator `/metrics`端点一样的信息。`/metrics`端点是以JSON格式输出当前度量信息的快照，而`metrics`命令则会接管Shell，以实时仪表盘的形式来显示结果。图7.3就是`metrics`命令的仪表盘。

![图7.3](../Figures/figure-7.3.png)

__Figure 7.3 The metrics dashboard__  
__图7.3 `metrics`命令的仪表盘__

It’s difficult to demonstrate the live dashboard behavior of the metrics command with a static figure in a book. But try to imagine that as memory, heap, and threads are consumed and released and as classes are loaded, the numbers shown in the dashboard will change to reflect the current values.  
很难在书里的一张静态图片里演示`metrics`命令的实时仪表盘。但你可以试着想象一下，内存、堆、线程在不断被消耗和释放，随着类的加载，仪表盘里显示的数量也会随之变化。

Once you’re finished looking at the metrics offered by the metrics command, press Ctrl-C to return to the shell.  
一旦看完了`metrics`命令提供的度量信息，按Ctrl-C就能回到Shell了。

### 7.2.4 Invoking Actuator endpoints
### 7.2.4 调用Actuator端点

You’ve probably realized by now that not all of the Actuator’s endpoints have corresponding commands in the shell. Does that mean that the shell can’t be a full replacement for the Actuator endpoints? Will you still have to query the endpoints directly for some of the internals offered by the Actuator? Although the shell doesn’t pair a command up with each of the endpoints, the endpoint command enables you to invoke Actuator endpoints from within the shell.  
你现在应该已经意识到了，并非所有的Actuator端点都有对应的Shell命令。那这是否意味着Shell不能完全代替Actuator端点呢？是否还是要直接查询这些端点来获取Actuator提供的内部信息呢？虽然Shell没能完全匹配上这些端点，但`endpoint`命令让你能在Shell里调用Actuator的端点。

First, you need to know which endpoint you want to invoke. You can get a list of endpoints by issuing endpoint list at the shell prompt, as shown in figure 7.4. Notice that the endpoints listed are referred to by their bean names, not by their URL paths.  
首先，你要知道自己想调用哪个端点。在Shell提示符中键入`endpoint list`就能获得端点的列表，如图7.4所示。请注意，列表中的端点用的是它们的Bean名称，而非URL路径。

![图7.4](../Figures/figure-7.4.png)

__Figure 7.4 Getting a list of endpoints__  
__图7.4 获得端点列表__

When you want to call one of the endpoints from the shell, you’ll use the endpoint invoke command, giving it the endpoint’s bean name without the “Endpoint” suffix. For example, to invoke the health endpoint, you’d issue endpoint invoke health at the shell prompt, as shown in figure 7.5.  
当你想在Shell里调用其中的某个端点时，可以使用`endpoint invoke`命令，传入不带“Endpoint”后缀的Bean名称。举例来说，要调用健康检查端点，在Shell提示符里键入`endpoint invoke health`，如图7.5所示。

![图7.5](../Figures/figure-7.5.png)

__Figure 7.5 Invoking the health endpoint__  
__图7.5 调用健康检查端点__

Notice that the results coming back from the endpoint are in the form of a raw, unformatted JSON document. Although it may be nice to be able to invoke the Actuator’s endpoints from within the shell, the results can be a bit difficult to read. Out of the box, there’s not much that can be done about that. But if you’re feeling adventurous, you can create a custom CRaSH shell command that accepts the unformatted JSON via a pipe and pretty-prints it. And you can always cut and paste it into a tool of your choosing for further review or formatting.  
请注意，这些端点返回的信息都是原始格式的，即未格式化过的JSON文档。虽然在Shell里调用Actuator的端点是不错，但输出结果很难阅读。就这个问题，自带的功能没什么能帮得上忙的。但如果你爱折腾，可以创建一个自定义的CRaSH Shell命令，通过管道接受未格式化过的JSON，然后美化输出。你总是可以剪切复制`endpoint`命令的输出，将其放入你喜欢的工具进行阅读或格式化。

## 7.3 Monitoring your application with JMX
## 7.3 通过JMX来监控你的应用程序

In addition to the endpoints and the remote shell, the Actuator also exposes its endpoints as MBeans to be viewed and managed through JMX (Java Management Extensions). JMX is an attractive option for managing your Spring Boot application, especially if you’re already using JMX to manage other MBeans in your applications.  
除了REST端点和远程Shell，Actuator还把它的端点以MBean的方式发布了出来，可以通过JMX（Java Management Extensions）来查看和管理。JMX是中不错的Spring Boot应用程序管理方式，尤其是当你已经在用JMX管理应用程序中的其他MBean的时候。

All of the Actuator’s endpoints are exposed under the org.springframework.boot domain. For example, suppose you want to view the request mappings for your application. Figure 7.6 shows the request mapping endpoint as viewed in JConsole.  
Actuator的端点都发布在org.springframework.boot域下，比如说，你想要查看应用程序的请求映射关系，图7.6就是通过JConsole来查看请求映射端点。

![图7.6](../Figures/figure-7.6.png)

__Figure 7.6 Request mapping endpoint as viewed in JConsole__  
__图7.6 通过JConsole查看请求映射端点__

As you can see, the request mapping endpoint is found under requestMappingEndpoint, which is under Endpoint in the org.springframework.boot domain. The Data attribute contains the JSON reported by the endpoint.  
如你所见，可以在`requestMappingEndpoint`下找到请求映射端点，位于org.springframework.boot域中的`Endpoint`下。`Data`属性中包含了该端点所要输出的JSON内容。

As with any MBean, the endpoint MBeans have operations that you can invoke. Most of the endpoint MBeans only have accessor operations that return the value of one of their attributes. But the shutdown endpoint offers a slightly more interesting (and destructive!) operation, as shown in figure 7.7  
和其他MBean一样，端点MBean的有可供调用的操作。大部分端点MBean只有访问操作，返回其中的某个属性，但关闭端点提供了一些有趣（同时具有毁灭性）的操作，如图7.7所示。

![图7.7](../Figures/figure-7.7.png)

__Figure 7.7 Shutdown button invokes the endpoint.__  
__图7.7 调用该端点的关闭按钮__

If you ever need to shut down your application (or just like living dangerously), the shutdown endpoint is there for you. As shown in figure 7.7, it’s waiting for you to click the “shutdown” button to invoke the endpoint. Be careful, though—there’s no turning back or “Are you sure?” prompt.  
如果你想要关闭应用程序（或者喜欢危险的生活），关闭应用的端点正合你意。如图7.7所示，这个界面就等着你点击“shutdown”按钮来调用该端点。请小心，这里没有“后悔药”，也没有“你确定吗？”之类的提示。

The very next thing you’ll see is shown in figure 7.8.  
接下来你要看到的东西如图7.8所示。

![图7.8](../Figures/figure-7.8.png)

__Figure 7.8 Application immediately shut__  
__图7.8 应用程序立马被关闭__

After that, your application will have been shut down. And because it’s dead, there’s no way it could possibly expose another MBean operation for restarting it. You’ll have to restart it yourself, the same way you started it in the first place.  
在那以后，你的应用程序就被关闭了。因为应用已经被关了，自然就没办法发布其他用来重启它的MBean操作了。你必须自己重启它，就和一开始的启动方式一样。

## 7.4 Customizing the Actuator
## 7.4 定制Actuator

Although the Actuator offers a great deal of insight into the inner workings of a running Spring Boot application, it may not be a perfect fit for your needs. Maybe you don’t need everything it offers and want to disable some of it. Or maybe you need to extend it with metrics custom-suited to your application.  
虽然Actuator提供了很多运行中的Spring Boot应用程序的内部工作细节，但难免还是会和你的需求有所偏差。也许你并不需要它提供的所有功能，想要关闭一些也说不定。或者你需要对Actuator稍作扩展，增加一些自定义的度量信息，以此满足你对应用程序的需求。

As it turns out, the Actuator can be customized in several ways, including the following:  
实际上，Actuator有多种定制方式，包括：

* Renaming endpoints
* Enabling and disabling endpoints
* Defining custom metrics and gauges
* Creating a custom repository for storing trace data
* Plugging in custom health indicators
* 重命名端点
* 启用和禁用端点
* 自定义度量信息
* 创建自定义仓库来存储跟踪数据
* 插入自定义的健康指示器

We’re going to see how to customize the Actuator, bending it to meet our needs. We'll start with one of the simplest customizations: renaming the Actuator's endpoints.  
接下来，我们会了解到如何定制Actuator，让它满足我们的需要。先来看一个最简单的定制：重命名Actuator端点。

### 7.4.1 Changing endpoint IDs
### 7.4.1 修改端点ID

Each of the Actuator endpoints has an ID that’s used to determine that endpoint’s path. For example, the /beans endpoint has beans as its default ID.  
每个Actuator端点都有一个ID用来决定端点的路径，比方说，`/beans`端点的默认ID就是`beans`。

If an endpoint’s path is determined by its ID, then it stands to reason that you can change an endpoint’s path by changing its ID. All you need to do is set a property whose name is endpoints.endpoint-id.id.  
如果端点的路径是由ID决定的，那么就可以通过修改ID来改变端点的路径。你要做的就是设置一个属性，属性名是`endpoints.endpoint-id.id`。

To demonstrate how this works, consider the /shutdown endpoint. It responds to POST requests sent to /shutdown. Suppose, however, that you’d rather have it handle POST requests sent to /kill. The following YAML shows how you might assign a new ID, and therefore a new path, to the /shutdown endpoint:  
我们用`/shutdown`端点来做个演示，它会响应发往`/shutdown`的POST请求。假设你想让它处理发往`/kill`的POST请求，可以通过如下YAML为其赋予一个新的ID，也就是新的路径：

```
endpoints:
  shutdown:
    id: kill
```

There are a couple of reasons you might want to rename an endpoint and change its path. The most obvious is that you might simply want to name the endpoints to match the terminology used by your team. But you might also think that renaming an endpoint will hide it from anyone who might be familiar with the default names, thus creating a sense of security by obscurity.  
你可能会有很多理由重命名端点，修改它的路径。最明显的理由就是你希望端点的命名能和团队的术语保持一致，你也可能想重命名端点，以便让那些熟悉默认名称的人找不到它，藉此增加一些安全感。

Unfortunately, renaming an endpoint doesn’t really secure it. At best, it will only slow down a hacker looking to gain access to an endpoint. We’ll look at how you can secure Actuator endpoints in section 7.5. For now, let’s see how to completely disable any (or all) endpoints that you don’t want anyone to have access to.  
不幸的是重命名段带你并不能真的起到保护作用，顶多是让黑客慢点找到它们。在7.5节里我们会看到如何保护这些Actuator端点的，现在先让我们来看看如何禁用某个（或全部）不希望别人访问的端点。

### 7.4.2 Enabling and disabling endpoints
### 7.4.2 启用和禁用端点

Although all of the Actuator endpoints are useful, you may not want or need all of them. By default, all of the endpoints (except for /shutdown) are enabled. We’ve already seen how to enable the /shutdown endpoint by setting endpoints.shutdown.enabled to true (in section 7.1.1). In the same way, you can disable any of the other endpoints by setting endpoints.endpoint-id.enabled to false.  
虽然Actuator的端点都很有用，但你不一定需要全部这些端点。默认情况下，所有端点（除了`/shutdown`）都是启用的。我们已经看过如何设置`endpoints.shutdown.enabled`为`true`，以此来开启`/shutdown`端点了（在7.1.1节）。用同样的方式，你可以禁用其他的端点，将`endpoints.endpoint-id.enabled `设置为`false`。

For example, suppose you want to disable the /metrics endpoint. All you need to do is set the endpoints.metrics.enabled property to false. In application.yml, that would look like this:  
例如，假设你要禁用`/metrics`端点，你要做的就是将`endpoints.metrics.enabled`属性设置为`false`。在application.yml里做如下设置：

```
endpoints:
  metrics:
    enabled: false
```

If you find that you only want to leave one or two of the endpoints enabled, it might be easier to disable them all and then opt in to the ones you want to enable. For example, consider the following snippet from application.yml:  
如果你只想打开一两个端点，先禁用全部端点，然后启用那几个你要的会更容易点。例如，考虑如下application.yml片段：

```
endpoints:
  enabled: false
  metrics:
    enabled: true
```

As shown here, all of the Actuator’s endpoints are disabled by setting endpoints.enabled to false. Then the /metrics endpoint is re-enabled by setting endpoints.metrics.enabled to true.  
正如以上片段所示，`endpoints.enabled`设置为`false`就能禁用全部Actuator的端点，然后将`endpoints.metrics.enabled`设置为`true`重新启用`/metrics`端点。

### 7.4.3 Adding custom metrics and gauges
### 7.4.3 添加自定义度量信息

In section 7.1.2, you saw how to use the /metrics endpoint to fetch information about the internal metrics of a running application, including memory, garbage collection, and thread metrics. Although these are certainly useful and informative metrics, you may want to define custom metrics to capture information specific to your application.  
在7.1.2节中，你看到了如何从`/metrics`端点获得运行中应用程序的内部度量信息，包括内存、垃圾回收和线程信息。虽然这些都是非常有用且信息量很大的度量值，但你可能还想定义自己的度量，用来捕获应用程序中的特定信息。

Suppose, for instance, that we want a metric that reports how many times a user has saved a book to their reading list. The easiest way to capture this number is to increment a counter every time the addToReadingList() method is called on ReadingListController. A counter is simple enough to implement, but how would you expose the running total along with the other metrics exposed by the /metrics endpoint?  
比方说，我们想要知道用户往阅读列表里保存了多少次图书，最简单的方法就是在每次调用`ReadingListController`的`addToReadingList()`方法时增加计数器值。计数器太容易实现了，但如何同那些`/metrics`端点发布出的度量信息一起将这个不断变化的总计值发布出来呢？

Let’s also suppose that we want to capture a timestamp for the last time a book was saved. We could easily capture that by calling System.currentTimeMillis(), but how could we report that time in the /metrics endpoint?  
再假设我们想要获得最后保存图书的时间戳。可以通过调用`System.currentTimeMillis()`来获取时间戳，但如何在`/metrics`端点里报告该时间戳呢？

As it turns out, the auto-configuration that enables the Actuator also creates an instance of CounterService and registers it as a bean in the Spring application context. CounterService is an interface that defines three methods for incrementing, decrementing, or resetting a named metric, as shown here:  
实际上，自动配置允许Actuator创建`CounterService`的实例，并将其注册到Spring的应用程序上下文里。`CounterService`这个接口里定义了三个方法，分别用来增加、减少或重置特定名称的度量值，代码如下所示：

```
package org.springframework.boot.actuate.metrics;

public interface CounterService {
  void increment(String metricName);
  void decrement(String metricName);
  void reset(String metricName);
}
```

Actuator auto-configuration will also configure a bean of type GaugeService, an interface similar to CounterService that lets you record a single value to a named gauge metric. GaugeService looks like this:  
Actuator的自动配置还会配置一个`GaugeService`类型的Bean，该接口与`CounterService`类似，让你能将某个值记录到特定名称的度量值里。`GaugeService`看起来是这样的：

```
package org.springframework.boot.actuate.metrics;

public interface GaugeService {
  void submit(String metricName, double value);
}
```

We don’t need to implement either of these interfaces; Spring Boot already provides implementations for them both. All we must do is inject the CounterService and GaugeService instances into any other bean where they’re needed, and call the methods to update whichever metrics we want.  
你无需实现这些接口；Spring Boot已经为你提供了两者的实现。我们所要做的就是把它们的实例注入到所需的Bean里，在适当的时候调用其中的方法更新度量值就可以了。

For the metrics we want, we’ll need to inject the CounterService and GaugeService beans into ReadingListController and call their methods from the addToReadingList() method. Listing 7.9 shows the necessary changes to ReadingListController.  
针对上文提到的需求，我们需要把`CounterService`和`GaugeService` Bean注入到`ReadingListController`里，然后在`addToReadingList()`方法里调用其中的方法。代码7.9是`ReadingListController`里的相关变动：

__Listing 7.9 Using injected gauge and counter services__
__代码7.9 使用注入的`CounterService`和`GaugeService`__

```
@Controller
@RequestMapping("/")
@ConfigurationProperties("amazon")
public class ReadingListController {
  ...
  private CounterService counterService;

  @Autowired
  public ReadingListController(
      ReadingListRepository readingListRepository,
      AmazonProperties amazonProperties,
      CounterService counterService,
      GaugeService gaugeService) {
    this.readingListRepository = readingListRepository;
    this.amazonProperties = amazonProperties;
    this.counterService = counterService;
    this.gaugeService = gaugeService;
  }

  ...

  @RequestMapping(method=RequestMethod.POST)
  public String addToReadingList(Reader reader, Book book) {
    book.setReader(reader);
    readingListRepository.save(book);
    counterService.increment("books.saved");
    gaugeService.submit(
      "books.last.saved", System.currentTimeMillis());
    return "redirect:/";
  }
}
```

Inject the counter and gauge services  
注入`CounterService`和`GaugeService`

Increment “books.saved”  
增加“books.saved”的值

Record “books.last.saved”  
记录“books.last.saved”的值

This change to ReadingListController uses autowiring to inject the CounterService and GaugeService beans via the controller’s constructor, which then stores them in instance variables. Then, each time that the addToReadingList() method handles a request, it will call counterService.increment("books.saved") and gaugeService.submit("books.last.saved") to adjust our custom metrics.  
修改后的`ReadingListController`使用了自动织入机制，通过控制器的构造方法注入`CounterService`和`GaugeService`，随后把它们保存在实例变量里。后续在`addToReadingList()`方法每次处理请求时就会调用`counterService.increment("books.saved")`和`gaugeService.submit("books.last.saved")`来调整度量值。

Although CounterService and GaugeService are simple to use, there are some metrics that are hard to capture by incrementing a counter or recording a gauge value. For those cases, we can implement the PublicMetrics interface and provide as many custom metrics as we want. The PublicMetrics interface defines a single metrics() method that returns a collection of Metric objects:  
尽管`CounterService`和`GaugeService`用起来很简单，但还是有一些度量值很难通过增加计数器或记录指标值来捕获。对于那些情况，我们可以实现`PublicMetrics`接口，提供自己需要的度量信息。该接口定义了一个`metrics()`方法，返回一个`Metric`对象的集合：

```
package org.springframework.boot.actuate.endpoint;

public interface PublicMetrics {
  Collection<Metric<?>> metrics();
}
```

To put PublicMetrics to work, suppose that we want to be able to report some metrics from the Spring application context. The time when the application context was started and the number of beans and bean definitions might be interesting metrics to include. And, just for grins, let’s also report the number of beans that are annotated as @Controller. Listing 7.10 shows the implementation of PublicMetrics that will do the job.  
要了解`PublicMetrics`的使用方法，假设我们想报告一些源自Spring应用程序上下文里的度量值——应用程序上下文启动的时间、Bean及Bean定义的数量，还要报告添加了`@Controller`注解的Bean的数量。代码7.10给出了相应`PublicMetrics`实现的代码。

__Listing 7.10 Publishing custom metrics__  
__代码7.10 发布自定义度量信息__

```
package readinglist;
import java.util.ArrayList;
import java.util.Collection;
import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.actuate.endpoint.PublicMetrics;
import org.springframework.boot.actuate.metrics.Metric;
import org.springframework.context.ApplicationContext;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Controller;

@Component
public class ApplicationContextMetrics implements PublicMetrics {
  private ApplicationContext context;

  @Autowired
  public ApplicationContextMetrics(ApplicationContext context) {
    this.context = context;
  }

  @Override
  public Collection<Metric<?>> metrics() {
    List<Metric<?>> metrics = new ArrayList<Metric<?>>();
    metrics.add(new Metric<Long>("spring.context.startup-date",
        context.getStartupDate()));

    metrics.add(new Metric<Integer>("spring.beans.definitions",
        context.getBeanDefinitionCount()));

    metrics.add(new Metric<Integer>("spring.beans",
        context.getBeanNamesForType(Object.class).length));

    metrics.add(new Metric<Integer>("spring.controllers",
        context.getBeanNamesForAnnotation(Controller.class).length));

    return metrics;
  }
}
```

Record startup date  
记录启动时间

Record bean definition count  
记录Bean定义数量

Record bean count  
记录Bean数量

Record controller bean count  
记录控制器类型的Bean数量

The metrics() method will be called by the Actuator to get any custom metrics that ApplicationContextMetrics provides. It makes a handful of calls to methods on the injected ApplicationContext to fetch the numbers we want to report as metrics. For each one, it creates an instance of Metric, specifying the metric’s name and the value, and adds the Metric to the list to be returned.  
Actuator会调用`metrics()`方法来收集`ApplicationContextMetrics`提供的度量信息。该方法调用了所注入的`ApplicationContext`上的方法来获取我们想要报告的数量。每个度量值都会创建一个`Metrics`实例，指定度量的名称和值，将其加入要返回的列表里。

As a consequence of creating ApplicationContextMetrics as well as using CounterService and GaugeService in ReadingListController, we get the following entries in the response from the /metrics endpoint:  
在创建了`ApplicationContextMetrics`，并在`ReadingListController`里使用了`CounterService`和`GaugeService`后，我们能在`/metrics`端点的响应中找到如下条目：

```
{
  ...
  spring.context.startup-date: 1429398980443,
  spring.beans.definitions: 261,
  spring.beans: 272,
  spring.controllers: 2,
  books.count: 1,
  gauge.books.save.time: 1429399793260,
  ...
}
```

Of course, the actual values for these metrics will vary, depending on how many books you’ve added and the times when you started the application and last saved a book. In case you’re wondering, spring.controllers is 2 because it’s counting ReadingListController as well as the Spring Boot–provided BasicErrorController.  
当然，这些度量的实际值会根据添加了多少书、何时启动应用程序及何时保存最后一本书而发生变化。在这个例子里，你一定会好奇，为什么`spring.controllers`是2。因为其中算上了`ReadingListController`以及Spring Boot提供的`BasicErrorController`。

### 7.4.4 Creating a custom trace repository
### 7.4.4 创建自定义跟踪仓库

By default, the traces reported by the /trace endpoint are stored in an in-memory repository that’s capped at 100 entries. Once it’s full, it starts rolling off older trace entries to make room for new ones. This is fine for development purposes, but in a production application the higher traffic may result in traces being discarded before you ever get a chance to see them.  
默认情况下，`/trace`端点报告的跟踪信息都是存储在内存仓库里的，封顶100个条目。一旦仓库满了，就开始移除老的条目，给新的条目腾出空间。在开发阶段这没什么问题，但在生产环境中，大流量会造成跟踪信息还没来得及看就被丢弃了。

One way to remedy that problem is to declare your own InMemoryTraceRepository bean and set its capacity to some value higher than 100. The following configuration class should increase the capacity to 1000 entries:  
为了避免这个问题，可以声明自己的`InMemoryTraceRepository` Bean，将它的容量调整至100以上。如下配置类可以将容量调整至1000个条目：

```
package readinglist;
import org.springframework.boot.actuate.trace.InMemoryTraceRepository;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ActuatorConfig {
  @Bean
  public InMemoryTraceRepository traceRepository() {
    InMemoryTraceRepository traceRepo = new InMemoryTraceRepository();
    traceRepo.setCapacity(1000);
    return traceRepo;
  }
}
```

Although a tenfold increase in the repository’s capacity should keep a few of those trace entries around a bit longer, a sufficiently busy application might still discard traces before you get a chance to review them. And because this is an in-memory trace repository, we should be careful about increasing the capacity too much, as it will have an impact on our application’s memory footprint.  
仓库容量翻了十倍，应该能让跟踪信息保存的时间更久一点，但一个足够繁忙的应用程序可能还是会在你查看这些信息前将其丢弃。因为这是一个内存存储的仓库，还要避免容量增长太多，这会影响应用程序的内存使用情况。

Alternatively, we could store those trace entries elsewhere—somewhere that’s not consuming memory and that will be more permanent. All we need to do is implement Spring Boot’s TraceRepository interface:  
除了上述方法，我们还可以将那些跟踪条目存储在其他地方——既不消耗内存，又能长久保存的地方。只需实现Spring Boot的`TraceRepository`接口即可：

```
package org.springframework.boot.actuate.trace;
import java.util.List;
import java.util.Map;

public interface TraceRepository {
  List<Trace> findAll();
  void add(Map<String, Object> traceInfo);
}
```

As you can see, TraceRepository only requires that we implement two methods: one that finds all stored Trace objects and another that saves a Trace given a Map containing trace information.  
如你所见，`TraceRepository`只要求我们实现两个方法：一个是查找所有存储的`Trace`对象，另一个是保存包含了跟踪信息的`Map`对象。

For demonstration purposes, perhaps we could create an instance of TraceRepository that stores trace entries in a MongoDB database. Listing 7.11 shows such an implementation of TraceRepository.  
为了进行演示，假设我们创建了一个使用MongoDB存储跟踪信息的`TraceRepository`实例。代码7.11演示了如何实现这个`TraceRepository`。

__Listing 7.11 Saving trace data to Mongo__  
__代码7.11 往MongoDB保存跟踪数据__

```
package readinglist;
import java.util.Date;
import java.util.List;
import java.util.Map;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.actuate.trace.Trace;
import org.springframework.boot.actuate.trace.TraceRepository;
import org.springframework.data.mongodb.core.MongoOperations;
import org.springframework.stereotype.Service;

@Service
public class MongoTraceRepository implements TraceRepository {

  private MongoOperations mongoOps;

  @Autowired
  public MongoTraceRepository(MongoOperations mongoOps) {
    this.mongoOps = mongoOps;
  }

  @Override
  public List<Trace> findAll() {
    return mongoOps.findAll(Trace.class);
  }

  @Override
  public void add(Map<String, Object> traceInfo) {
    mongoOps.save(new Trace(new Date(), traceInfo));
  }
}
```

Inject MongoOperations  
注入`MongoOperations`

Fetch all trace entries  
获取所有跟踪条目

Save a trace entry  
保存一个跟踪条目

The findAll() method is straightforward enough, asking the injected MongoOperations to find all Trace objects. The add() method is only slightly more interesting, instantiating a Trace object given the current date/time and the Map of trace info before saving it via MongoOperations.save(). The only question you might have is where MongoOperations comes from.  
`findAll()`方法很直白，用注入的`MongoOperations`来查找全部`Trace`对象。`add()`方法稍微有趣一点，用当前时间和含有跟踪信息的`Map`创建了一个`Trace`对象，通过`MongoOperations.save()`将其保存了下来。唯一的问题是`MongoOperations`是哪里来的？

In order for MongoTraceRepository to work, we’re going to need to make sure that we have a MongoOperations bean in the Spring application context. Thanks to Spring Boot starters and auto-configuration, that’s simply a matter of adding the MongoDB starter as a dependency. The Gradle dependency you need is as follows:  
为了能使用`MongoTraceRepository`，我们需要保证Spring应用程序上下文里先有一个`MongoOperations` Bean。感谢Spring Boot的起步依赖和自动配置，这一切就只需简单地添加MongoDB起步依赖即可。你需要如下的Gradle依赖：

```
compile("org.springframework.boot:spring-boot-starter-data-mongodb")
```

If your project is built with Maven, this is the dependency you’ll need:  
如果你用的是Maven，需要如下依赖：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

By adding this starter, Spring Data MongoDB and supporting libraries will be added to the application’s classpath. And because those are in the classpath, Spring Boot will auto-configure the beans necessary to support working with a MongoDB database, including a MongoOperations bean. The only other thing you’ll need to do is be sure that there’s a MongoDB server running for MongoOperations to talk to.  
添加了这个起步依赖后，Spring Data MongoDB和所依赖的库会被添加到应用程序的Classpath里，Spring Boot会自动配置所需的Bean，以便能够使用MongoDB数据库，这些Bean里就包括了`MongoOperations`。另外，你需要确保和`MongoOperations`通讯的MongoDB服务器是正常运行的。

### 7.4.5 Plugging in custom health indicators
### 7.4.5 插入自定义健康指示器

As we’ve seen, the Actuator comes with a nice set of out-of-the-box health indicators for common needs such as reporting the health of a database or message broker that the application is using. But what if your application interacts with some system for which there’s no health indicator?  
如前文所述，Actuator自带了很多健康指示器，能共满足常见需求，比如报告应用程序使用的数据库和消息代理的健康情况。但如果你的应用程序需要和一些没有健康指示器的系统交互该怎么办呢？

Because our application includes links to Amazon for books in the reading list, it might be interesting to report whether or not Amazon is reachable. Sure, it’s not likely that Amazon will go down, but stranger things have happened. So let’s create a health indicator that reports whether Amazon is available. Listing 7.12 shows a HealthIndicator implementation that should do the job.  
因为我们的阅读列表里有指向Amazon的图书链接，可以报告一下Amazon是否可以访问。当然，Amazon不太可能宕机，但不怕一万就怕万一，所以让我们为Amazon创建一个健康指示器吧。代码7.12演示了相关`HealthIndicator`的实现。

__Listing 7.12 Defining a custom Amazon health indicator__  
__代码7.12 自定义一个Amazon健康指示器__

```
package readinglist;
import org.springframework.boot.actuate.health.Health;
import org.springframework.boot.actuate.health.HealthIndicator;
import org.springframework.stereotype.Component;
import org.springframework.web.client.RestTemplate;

@Component
public class AmazonHealth implements HealthIndicator {

  @Override
  public Health health() {
    try {
      RestTemplate rest = new RestTemplate();
      rest.getForObject("http://www.amazon.com", String.class);
      return Health.up().build();
    } catch (Exception e) {
      return Health.down().build();
    }
  }

}
```

Send request to Amazon  
向Amazon发送请求

Report “down” health  
报告“DOWN”状态

The AmazonHealth class isn’t terribly fancy. The health() method simply uses Spring’s RestTemplate to perform a GET request to Amazon’s home page. If it works, it returns a Health object indicating that Amazon is “UP”. On the other hand, if an exception is thrown while requesting Amazon’s home page, then health() returns a Health object indicating that Amazon is “DOWN”.  
`AmazonHealth`类并没有什么花哨的地方，`health()`方法里简单地使用了Spring的`RestTemplate`向Amazon首页发起了一个GET请求。如果请求成功，则返回一个表明Amazon状态为“UP”的`Health`对象。如果请求发生异常，则`health()`返回一个标明Amazon状态为“DOWN”的`Health`对象。

The following excerpt from the /health endpoint’s response shows what you might see if Amazon is unreachable:  
下面是`/health`端点响应的一个片段，从中可以看到Amazon是否可以访问：

```
{
  "amazonHealth": {
    "status": "DOWN"
  },
  ...
}
```

You wouldn’t believe how long I had to wait for Amazon to crash so that I could get that result!1  
你不会相信为了等Amazon宕机，我等了有多久，就为了能看到上面的结果！<sup>[1][]</sup>

If you’d like to add additional information to the health record beyond a simple status, you can do so by calling withDetail() on the Health builder. For example, to add the exception’s message as a reason field in the health record, the catch block could be changed to return a Health object created like this:  
除了简单的状态之外，如果你还想向健康记录里添加其他附加信息，可以调用`Health`构造器的`withDetail()`方法。例如，要添加异常消息，将其作为健康记录的`reason`字段，可以对`catch`块中的返回语句做如下修改：

```
return Health.down().withDetail("reason", e.getMessage()).build();
```

As a result of this change, the health record might look like this when the request to Amazon fails:  
修改后，当Amazon无法访问时，健康记录看起来是这样的：

```
"amazonHealth": {
  "reason": "I/O error on GET request for
             \"http://www.amazon.com\":www.amazon.com;
             nested exception is java.net.UnknownHostException:
             www.amazon.com",
  "status": "DOWN"
},
```

You can add as many additional details as you want by calling withDetail() for each additional field you want included in the health record.  
如果你有很多附加信息，可以多次调用`withDetail()`方法，每次设置一个想要放入健康记录的附加字段。

[1]: # "Not really. I just disconnected my computer from the network. No network, no Amazon. 实际上我并没有等太久，我只是把电脑的网络断开了。没有网就没有Amazon。"

## 7.5 Securing Actuator endpoints  
## 7.5 保护Actuator端点

We’ve seen that many of the Actuator endpoints expose information that may be considered sensitive. And some, such as the /shutdown endpoint, are dangerous and can be used to bring your application down. Therefore, it’s very important to be able to secure these endpoints so that they’re only available to authorized clients.  
很多Actuator端点发布的信息都可能涉及敏感数据，还有一些端点，比如`/shutdown`则非常危险，可以用来关闭应用程序。因此，保护这些端点就变得尤为重要了，能访问它们的就只能是那些经过授权的客户端。

As it turns out, the Actuator endpoints can be secured the same way as any other URL path: with Spring Security. In a Spring Boot application, this means adding the Security starter as a build dependency and letting security auto-configuration take care of locking down the application, including the Actuator endpoints.  
实际上，可以用和其他URL路径一样的方式来保护Actuator的端点——使用Spring Security。在Spring Boot应用程序中，这意味着加入Security起步依赖，然后就让安全相关的自动配置来保护应用程序即可，其中当然也包括了Actuator端点。

In chapter 3, we saw how the default security auto-configuration results in all URL paths being secured, requiring HTTP Basic authentication where the username is “user” and the password is randomly generated at startup and written to the log file. This was not how we wanted to secure the application, and it’s likely not how you want to secure the Actuator either.  
在第3章里，我们看到了默认安全自动配置是如何把所有URL路径保护起来的，要求HTTP基本身份验证，用户名是“user”，密码是启动时随机生成并写到日志文件里去的。这不是我们希望的应用程序保护方式，也不是你所希望的Actuator保护方式。

We’ve already added some custom security configuration to restrict the root URL path (/) to only authenticated users with READER access. To lock down Actuator endpoints, we’ll need to make a few changes to the configure() method in SecurityConfig.java.  
我们已经添加了一些自定义安全配置，限制了根URL路径（/）的访问，只有带了READER权限的授权用户才能访问。要保护Actuator的端点，我们需要对`SecurityConfig.java`的`configure()`方法做些修改。

Suppose, for instance, that we want to lock down the /shutdown endpoint, requiring that the user have ADMIN access. Listing 7.13 shows the changes required in the configure() method.  
举例来说，你想要保护`/shutdown`端点，让拥有ADMIN权限的用户才能访问它。代码7.13就是新的`configure()`方法。

__Listing 7.13 Securing the /shutdown endpoint__  
__代码7.13 保护`/shutdown`端点__

```
@Override
protected void configure(HttpSecurity http) throws Exception {
  http
    .authorizeRequests()
      .antMatchers("/").access("hasRole('READER')")
      .antMatchers("/shutdown").access("hasRole('ADMIN')")
      .antMatchers("/**").permitAll()
    .and()
    .formLogin()
      .loginPage("/login")
      .failureUrl("/login?error=true");
}
```

Require ADMIN access  
要求有ADMIN权限

Now the only way to access the /shutdown endpoint is to authenticate as a user with ADMIN access.  
现在要访问`/shutdown`端点，必须用一个带ADMIN权限的用户来做身份验证。

The custom UserDetailsService we created in chapter 3, however, is coded to only apply READER access to users it looks up via the ReaderRepository. Therefore, you may want to create a smarter UserDetailsService implementation that is able to apply ADMIN access to some users. Optionally, you can configure an additional authentication implementation, such as the in-memory authentication shown in listing 7.14.  
然而，第3章里的自定义`UserDetailsService`只对通过`ReaderRepository`加载上来的用户赋予了READER权限，因此你需要创建一个更聪明的`UserDetailsService`实现，对某些用户赋予ADMIN权限。为此，可以配置一个额外的身份验证实现，比如代码7.14里的内存实现。

__Listing 7.14 Adding an in-memory admin authentication user__  
__代码7.14 添加一个内存里的admin用户__

```
@Override
protected void configure(
            AuthenticationManagerBuilder auth) throws Exception {
  auth
    .userDetailsService(new UserDetailsService() {
      @Override
      public UserDetails loadUserByUsername(String username)
          throws UsernameNotFoundException {
        UserDetails user = readerRepository.findOne(username);
        if (user != null) {
          return user;
        }
        throw new UsernameNotFoundException(
                      "User '" + username + "' not found.");
      }
    })
    .and()
    .inMemoryAuthentication()
      .withUser("admin").password("s3cr3t")
                        .roles("ADMIN", "READER");
}
```

Reader authentication
Reader身份验证

Admin authentication
Admin身份验证

With the in-memory authentication added, you can authenticate with “admin” as the username and “s3cr3t” as the password and be granted both ADMIN and READER access.  
在新加的内存身份验证中，将用户名定义为“admin”，密码为“s3cr3t”，同时授予了ADMIN和READER权限。

Now the /shutdown endpoint is locked down for everyone except users with ADMIN access. But what about the Actuator’s other endpoints? Assuming you want to lock them down with ADMIN access as for /shutdown, you can list each of them in the call to antMatchers(). For example, to lock down /metrics and /configprops as well as /shutdown, call antMatchers() like this:  
现在谁都无法访问`/shutdown`端点，除了那些拥有ADMIN权限的用户。但Actuator的其他端点呢？假设你想像`/shutdown`一样只让有ADMIN的用户访问它们，可以在`antMatchers()`里列出这些URL。例如，要保护`/metrics`、`/confiprops`和`/shutdown`，可以像这样来调用`antMatchers()`：

```
.antMatchers("/shutdown", "/metrics", "/configprops")
                          .access("hasRole('ADMIN')")
```

Although this approach will work, it’s only suitable if you want to secure a small subset of the Actuator endpoints. It becomes unwieldy if you use it to lock down all of the Actuator’s endpoints.  
虽然这么做能奏效，但也只适用于保护少数Actuator端点的时候。如果要保护全部Actuator端点，这种做法就不太方便了。

Rather than explicitly list all of the Actuator endpoints when calling antMatchers(), it’s much easier to use wildcards to match them all with a simple Ant-style expression. This is challenging, however, because there’s not a lot in common between the endpoint paths. And we can’t apply ADMIN access to “/\*\*” because then everything except for the root path (/) would require ADMIN access.  
比起在`antMatchers()`方法里显式地列出所有的Actuator端点，用通配符在一个简单的Ant风格表达式里匹配全部的Actuator端点则更容易一些。但是，这么做也有点小挑战，因为不同的端点之间没有什么共同点。我们也不能在“/\*\*”上运用ADMIN权限，因为这么一来除了根路径（/）之外都会要求有ADMIN权限。

Instead, consider setting the endpoint’s context path by setting the management.context-path property. By default, this property is empty, which is why all of the Actuator’s endpoint paths are relative to the root path. But the following entry in application.yaml will prefix them all with /mgmt.  
为此，可以通过`management.context-path`属性来设置端点的上下文路径。默认情况下，这个属性是空的，所以Actuator的端点都是相对于根路径的。在application.yaml里增加如下内容，可以让这些端点都带上`/mgmt`前缀。

```
management:
  context-path: /mgmt
```

Optionally, you can set it in application.properties like this:  
你也可以在application.properties里做类似的事情：

```
management.context-path=/mgmt
```

With management.context-path set to /mgmt, all Actuator endpoints will be relative to the /mgmt path. For example, the /metrics endpoint will be at /mgmt/metrics.  
将`management.context-path`设置为`/mgmt`后，所有的Actuator端点都会相对于/mgmt路径。例如，`/metrics`端点的URL会变为`/mgmt/metrics`。

With this new path, we now have a common prefix to work with when assigning ADMIN access restriction to the Actuator endpoints:  
有了这个新的路径，我们就有了公共的前缀，在为Actuator端点赋予ADMIN权限限制时就能借助这个公共前缀：

```
.antMatchers("/mgmt/**").access("hasRole('ADMIN')")
```

Now all requests beginning with /mgmt, which includes all Actuator endpoints, will require an authenticated user who has been granted ADMIN access.  
现在所有以`/mgmt`开头的请求，其中包含了所有的Actuator端点，都会限制只有被授予了ADMIN权限的认证用户才能访问。

## 7.6 Summary
## 7.6 小结

It can be difficult to know for sure what’s going on inside a running application. Spring Boot’s Actuator opens a portal into the inner workings of a Spring Boot application, exposing components, metrics, and gauges to help understand what makes the application tick.  
想弄清楚正在运行的应用程序里正在发生什么，这是件很困难的事。Spring Boot的Actuator为你打开了一扇大门，深入Spring Boot应用程序的内部细节，它发布出来的组件、度量和指标能帮你理解应用程序的运作情况。

In this chapter, we started by looking at the Actuator’s web endpoints—REST endpoints that expose runtime details over HTTP. These include endpoints for viewing all of the beans in the Spring application context, auto-configuration decisions, Spring MVC mappings, thread activity, application health, and various metrics, gauges, and counters.  
本章中，我们先了解了Actuator的Web端点——通过HTTP发布运行时细节信息的REST端点。这些端点的功能包含了查看Spring应用程序上下文里所有的Bean、查看自动配置决策、查看Spring MVC映射、查看线程活动、查看应用程序健康信息，还有多种度量、指标和计数器。

In addition to web endpoints, the Actuator also offers two alternative ways to dig into the information it provides. The remote shell offers a way to securely shell into the application itself and issue commands that expose much of the same data as the Actuator’s endpoints. Meanwhile, all of the Actuator’s endpoints are exposed as MBeans that can be monitored and managed by a JMX client.  
除了Web端点，Actuator还提供了另外两种获取它所提供的信息的途径。远程Shell让你能在Shell里安全地连上应用程序，发起指令，获得与Actuator端点相同的数据。与此同时，所有的Actuator端点也都发布成了MBean，可以通过JMX客户端进行监控和管理。

Next, we took a look at how to customize the Actuator. We saw how to change Actuator endpoint paths by changing the endpoint IDs as well as how to enable and disable endpoints. We also plugged in a few custom metrics and created a custom trace repository to replace the default in-memory trace repository.  
随后我们还了解了如何定制Actuator，包括如何通过端点的ID来修改Actuator端点的路径，如何启用和禁用端点等等。我们还插入了一些定制的度量信息，创建了定制的跟踪信息仓库，替换了原来的内存实现。

Finally, we looked at how to secure the Actuator’s endpoints, restricting access to authorized users.  
最后，我们学习了如何保护Actuator的端点，只让经过授权的用户访问它们。

Coming up in the next chapter, we’ll see how to take an application from the coding phase to production, looking at how Spring Boot helps when deploying an application to a variety of platforms, including traditional application servers and the cloud.  
接下来，在一章里，我们将看到如何让应用程序从编码阶段过渡到生产阶段，了解Spring Boot是如何协助我们在多种不同的平台上进行部署的，包括传统的应用容器和云平台。
