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

Personally, I’ve never had this concern. Maybe it’s because I realize that before Spring came along there wasn’t any map of the components in my applications.

Nevertheless, if it concerns you that auto-configuration hides how beans are wired up in the Spring application context, then I have some good news! The Actuator has endpoints that give you that missing application component map as well as some insight into the decisions that auto-configuration made when populating the Spring application context.

#### GETTING A BEAN WIRING REPORT

The most essential endpoint for exploring an application’s Spring context is the /beans endpoint. This endpoint returns a JSON document describing every single bean in the application context, its Java type, and any of the other beans it’s injected with. By performing a GET request to /beans (http://localhost:8080/beans when running locally), you’ll be given information similar to what’s shown in the following listing.

__Listing 7.1 The /beans endpoint exposes the beans in the Spring application context__

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

Resource file

Dependencies

Bean scope

Java type

Listing 7.1 is an abridged listing of the beans from the reading-list application. As you can see, all of the bean entries carry five pieces of information about the bean:

* bean—The name or ID of the bean in the Spring application context
* resource—The location of the physical .class file (often a URL into the built JAR file, but this might vary depending on how the application is built and run)
* dependencies—A list of bean IDs that this bean is injected with
* scope—The bean’s scope (usually singleton, as that is the default scope)
* type—The bean’s Java type

Although the beans report doesn’t draw a specific picture of how the beans are wired together (for example, via properties or constructor arguments), it does help you visualize the relationships of the beans in the application context. Indeed, it would be reasonably easy to write a utility that processes the beans report and produces a graphical representation of the bean relationships. Be aware, however, that the full bean report includes many beans, including many auto-configured beans, so such a graphic could be quite busy.

#### EXPLAINING AUTO-CONFIGURATION

Whereas the /beans endpoint produces a report telling you what beans are in the Spring application context, the /autoconfig endpoint might help you figure out why they’re there—or not there.

As mentioned in chapter 2, Spring Boot auto-configuration is built upon Spring conditional configuration. It provides several configuration classes with @Conditional annotations referencing conditions that decide whether or not beans should be automatically configured. The /autoconfig endpoint provides a report of all the conditions that are evaluated, grouping them by which conditions passed and which failed.

Listing 7.2 shows an excerpt from the auto-configuration report produced for the reading-list application with one passing and one failing condition.

__Listing 7.2 An auto-configuration report for the reading-list app__

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

Failed conditions

In the positiveMatches section, you’ll find a condition used to decide whether or not Spring Boot should auto-configure a JdbcTemplate bean. The match is named DataSourceAutoConfiguration.JdbcTemplateConfiguration#jdbcTemplate, which indicates the specific configuration class where this condition is applied. The type of condition is an OnBeanCondition, which means that the condition’s outcome is determined by the presence or absence of a bean. In this case, the message property makes it clear that the condition checks for the absence of a bean of type JdbcOperations (the interface that JbdcTemplate implements). If no such bean has already been configured, then this condition passes and a JdbcTemplate bean will be created.

Similarly, under negativeMatches, there’s a condition that decides whether or not to configure an ActiveMQ. This decision is an OnClassCondition, and it hinges on the presence of ActiveMQConnectionFactory in the classpath. Because ActiveMQConnectionFactory isn’t in the classpath, the condition fails and ActiveMQ will not be auto-configured.

#### INSPECTING CONFIGURATION PROPERTIES

In addition to knowing how your application beans are wired together, you might also be interested in learning what environment properties are available and what configuration properties were injected on the beans.

The /env endpoint produces a list of all of the environment properties available to the application, whether they’re being used or not. This includes environment variables, JVM properties, command-line parameters, and any properties provided in an application.properties or application.yml file.

The following listing shows an abridged example of what you might get from the /env endpoint.

__Listing 7.3 The /env endpoint reports all properties available__

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

Environment variables

JVM system properties

Essentially, any property source that can provide properties to a Spring Boot application will be listed in the results of the /env endpoint along with the properties provided by that endpoint.

In listing 7.3, properties come from application configuration (application.yml), Spring profiles, servlet context initialization parameters, the system environment, and JVM system properties. (In this case, there are no profiles or servlet context initialization parameters.)

It’s common to use properties to carry sensitive information such as database or API passwords. To keep that kind of information from being exposed by the /env endpoint, any property named (or whose last segment is) “password”, “secret”, or “key” will be rendered as “” in the response from /env. For example, if there’s a property named “database.password”, it will be rendered in the /env response like this:

```
"database.password":"******"
```

The /env endpoint can also be used to request the value of a single property. Just append the property name to /env when making the request. For example, requesting /env/amazon.associate_id will yield a response of “habuma-20” (in plain text) when requested against the reading-list application.

As you’ll recall from chapter 3, these environment properties come in handy when using the @ConfigurationProperties annotation. Beans annotated with @ConfigurationProperties can have their instance properties injected with values from the environment. The /configprops endpoint produces a report of how those properties are set, whether from injection or otherwise. Listing 7.4 shows an excerpt from the configuration properties report for the reading-list application.
