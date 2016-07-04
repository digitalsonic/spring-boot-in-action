#第7章 深入Actuator

>本章内容

> * Actuator Web端点
* 调整Actuator
* 通过shell连入运行中的应用程序
* 保护Actuator

你有没有猜过包好的礼物盒里装的是什么东西？你会摇一摇，掂一掂，量一量，你甚至会执着于里面到底有什么。但打开盒子那一刻前，你没办法确认里面是什么。

运行中的应用程序就像礼物盒。你可以刺探它，作出合理的推测，猜测它的运行情况。但如何了解真实的情况呢？有没有一种办法能让你深入应用程序内部一窥究竟，了解它的行为，检查它的健康状况，甚至触发一些操作来影响应用程序呢？

在本章中，我们将了解Spring Boot的Actuator。它提供了很多生产级的特性，比如监控和度量Spring Boot应用程序。Actuator的这些特性可以通过众多REST端点、远程shell和JMX获得。我们先来看看Actuator的REST端点，这种最为人所熟知的使用方式提供了最完整的功能。

## 7.1 揭秘Actuator的端点

Spring Boot Actuator的关键特性是在应用程序里提供众多Web端点，通过它们了解应用程序运行时的内部状况。有了Actuator，你可以知道Bean在Spring应用程序上下文里是如何组装在一起的，掌握应用程序可以获取的环境属性信息，获取运行时度量信息的快照……

Actuator提供了13个端点，具体如表7-1所示。

__表7-1 Actuator的端点__

| HTTP方法 | 路径              | 描述                                                           |
|----------|-------------------|---------------------------------------------------------------|
| `GET`      | /autoconfig     | 提供了一份自动配置报告，记录哪些自动配置条件通过了，哪些没通过  |
| `GET`      | /configprops    | 描述配置属性（包含默认值）如何注入Bean                |
| `GET`      | /beans          | 描述应用程序上下文里全部的Bean，以及它们的关系             |
| `GET`      | /dump           | 获取线程活动的快照                                            |
| `GET`      | /env            | 获取全部环境属性                                              |
| `GET`      | /env/{name}     | 根据名称获取特定的环境属性值                                   |
| `GET`      | /health         | 报告应用程序的健康指标，这些值由`HealthIndicator`的实现类提供    |
| `GET`      | /info           | 获取应用程序的定制信息，这些信息由`info`打头的属性提供           |
| `GET`      | /mappings       | 描述全部的URI路径，以及它们和控制器（包含Actuator端点）的映射关系 |
| `GET`      | /metrics        | 报告各种应用程序度量信息，比如内存用量和HTTP请求计数             |
| `GET`      | /metrics/{name} | 报告指定名称的应用程序度量值                                    |
| `GET`      | /shutdown       | 关闭应用程序，要求`endpoints.shutdown.enabled`设置为`true`     |
| `GET`      | /trace          | 提供基本的HTTP请求跟踪信息（时间戳、HTTP头等）                  |

要启用Actuator的端点，只需在项目中引入Actuator的起步依赖即可。在Gradle构建说明文件里，这个依赖是这样的：

```
compile 'org.springframework.boot:spring-boot-starter-actuator'
```

对于Maven项目，引入的依赖是这样的：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

亦或你使用Spring Boot CLI，可以使用如下`@Grab`注解：

```
@Grab('spring-boot-starter-actuator')
```

无论Actuator是如何添加的，在应用程序运行时自动配置都会生效。Actuator会开启。

表7-1中的端点可以分为三大类：配置端点、度量端点和其他端点。让我们分别了解一下这些端点，从提供应用程序配置信息的端点看起。

### 7.1.1 查看配置明细

关于Spring组件扫描和自动织入，最常遭人抱怨的问题之一就是很难看到应用程序中的组件是如何装配起来的。Spring Boot自动配置让这个问题变得更严重，因为Spring的配置更少了。在有显式配置的情况下，你至少还能看到XML文件或者配置类，对Spring应用程序上下文里的Bean关系有个大概的了解。

我个人从不担心这个问题。也许是因为我意识到，在Spring出现之前，根本就没有应用程序组件的映射关系。

但是，如果你担心自动配置隐藏了Spring应用程序上下文中Bean的装配细节，那么我要告诉你一个好消息！Actuator有一些端点不仅可以显示组件映射关系，还可以告诉你自动配置在配置Spring应用程序上下文时做了哪些决策。

####1.获得Bean装配报告

要了解应用程序中Spring上下文的情况，最重要的端点就是/beans。它会返回一个JSON文档，描述上下文里每个Bean的情况，包括其Java类型以及注入的其他Bean。向/beans（在本地运行时是[http://localhost:8080/beans](http://localhost:8080/beans)）发起`GET`请求后，你会看到与代码清单7-1示例类似的信息。

__代码清单7-1 /beans端点提供的Spring应用程序上下文Bean信息__

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
>文字

>Bean ID：Bean ID

>Resource file：资源文件

>Dependencies：依赖

>Bean scope：Bean作用域

>Java type：Java类型

代码清单7-1是阅读列表应用程序Bean信息的一个片段。如你所见，所有的Bean条目都有五类信息。

* `bean`：Spring应用程序上下文中的Bean名称或ID。
* `resource`：.class文件的物理位置，通常是一个URL，指向构建出的JAR文件。这会随着应用程序的构建和运行方式发生变化。
* `dependencies`：当前Bean注入的Bean ID列表。
* `scope`：Bean的作用域（通常是单例，这也是默认作用域）。
* `type`：Bean的Java类型。

虽然Bean报告不用具体绘图告诉你Bean是如何装配的（例如，通过属性或构造方法），但它帮你直观地了解了应用程序上下文中Bean的关系。实际上，写出一个工具，把Bean报告处理一下，用图形化的方式来展现Bean关系，这并不难。请注意，完整的Bean报告会包含很多Bean，还有很多自动配置的Bean，画出来的图会非常复杂。

####2.详解自动配置

/beans端点产生的报告能告诉你Spring应用程序上下文里都有哪些Bean。/autoconfig端点能告诉你为什么会有这个Bean，或者为什么没有这个Bean。

正如第2章里说的，Spring Boot自动配置构建于Spring的条件化配置之上。它提供了众多带有`@Conditional`注解的配置类，根据条件决定是否要自动配置这些Bean。/autoconfig端点提供了一个报告，列出了计算过的所有条件，根据条件是否通过进行分组。

代码清单7-2是阅读列表应用程序自动配置报告里的一个片段，里面有一个通过的条件，还有一个没通过的条件。

__代码清单7-2 阅读列表应用程序的自动配置报告__

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
>文字

>Successful conditions：成功条件

>Failed conditions：失败条件

在`positiveMatches`里，你会看到一个条件，决定Spring Boot是否自动配置`JdbcTemplate` Bean。匹配到的名字是`DataSourceAutoConfiguration.JdbcTemplateConfiguration#jdbcTemplate`，这是运用了条件的具体配置类。条件类型是`OnBeanCondition`，意味着条件的输出是由某个Bean的存在与否来决定的。在本例中，`message`属性已经清晰地表明了该条件是检查是否有`JdbcOperations`类型（`JbdcTemplate`实现了该接口）的Bean存在。如果没有配置这种Bean，则条件成立，创建一个`JdbcTemplate` Bean。

与之类似，在`negativeMatches`里，有一个条件决定了是否要配置ActiveMQ。这是一个`OnClassCondition`，会检查Classpath里是否存在`ActiveMQConnectionFactory`。因为Classpath里没有这个类，条件不成立，所以不会自动配置ActiveMQ。

####3.查看配置属性

除了要知道应用程序的Bean是如何装配的，你可能还对能获取哪些环境属性，哪些配置属性注入了Bean里感兴趣。

/env端点会生成应用程序可用的所有环境属性的列表，无论这些属性是否用到。这其中包括环境变量、JVM属性、命令行参数，以及applicaition.properties或application.yml文件提供的属性。

代码清单7-3的示例代码是/env端点获取信息的一个片段。

__代码清单7-3 /env端点会报告所有可用的属性__

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
>文字

>Application properties：应用属性

>Environment variables：环境变量

>JVM system properties：JVM系统属性

基本上，任何能给Spring Boot应用程序提供属性的属性源都会列在/env的结果里，同时会显示具体的属性。

代码清单7-3中的属性来源有很多，包括应用程序配置（application.yml）、Spring Profile、Servlet上下文初始化参数、系统环境变量和JVM系统属性。（本例中没有Profile和Servlet上下文初始化参数。）

属性常用来提供诸如数据库或API密码之类的敏感信息。为了避免此类信息暴露到/env里，所有名为password、secret、key（或者名字中最后一段是这些）的属性在/env里都会加上“*”。举个例子，如果有一个属性名字是database.password，那么它在/env中的显示效果是这样的：
```
"database.password":"******"
```
/env端点还能用来获取单个属性的值，只需要在请求时在/env后加上属性名即可。举例来说，对阅读列表应用程序发起`/env/amazon.associate_id`请求，获得的结果是habuma-20（纯文本形式）。

回想第3章，通过`@ConfigurationProperties`注解可以很方便地使用这些环境属性。这些环境属性会注入带有`@ConfigurationProperties`注解的Bean的实例属性。/configprops端点会生成一个报告，说明这些属性是如何进行设置的（注入或其他方式）。代码清单7-4是阅读列表应用程序的配置属性报告片段。

__代码清单7-4 配置属性报告__

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
>文字

>Amazon configuration：Amazon配置

>Server configuration：服务器配置

片段中的第一个内容是我们在第3章里创建的`amazonProperties` Bean。报告显示它添加了`@ConfigurationProperties`注解，前缀为`amazon`。`associateId`属性设置为habuma-20。这是因为在application.yml里，我们把`amazon.associateId`属性设置成了habuma-20。

你还会看到一个`serverProperties`条目（前缀是`server`），还有一些属性。它们都有默认值，你也可以通过设置`server`前缀的属性来改变这些值。举例来说，你可以通过设置`server.port`属性来修改服务器监听的端口。

除了展现运行中应用程序的配置属性如何设置，这个报告也能作为一个快速参考指南，告诉你有哪些属性可以设置。例如，如果你不清楚怎么设置嵌入式Tomcat服务器的最大线程数，可以看一下配置属性报告，里面会有一条`server.tomcat.maxThreads`，这就是你要找的属性。

####4.生成端点到控制器的映射

在应用程序相对较小的时候，很容易搞清楚控制器都映射到了哪些端点上。如果Web界面的控制器和请求处理方法数量多，那最好能有一个列表，罗列出应用程序发布的全部端点。

/mappings端点就提供了这么一个列表。代码清单7-5是阅读列表应用程序的映射报告片段。

**代码清单7-5 阅读列表应用程序的控制器/端点映射**

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
>文字

>ReadingListController mappings：`ReadingListController`映射

>Auto-configuration report mapping：自动配置报告的映射

这里我们可以看到不少端点的映射。每个映射的键都是一个字符串，其内容就是Spring MVC的`@RequestMapping`注解上设置的属性。实际上，这个字符串能让你清晰地了解控制器是如何映射的，哪怕不看源代码。每个映射的值都有两个属性：`bean`和`method`。`bean`属性标识了Spring Bean的名字，映射源自这个Bean。`method`属性是映射对应方法的全限定方法签名。

头两个映射关乎应用程序中`ReadingListController`的请求如何处理。第一个表明`readersBooks()`方法处理根路径（“/”）的HTTP `GET`请求。第二个表明`POST`请求映射到`addToReadingList()`方法上。

接下来的映射是Actuator提供的端点。/autoconfig端点的HTTP `GET`请求由Spring Boot的`EndpointMvcAdapter`类的`invoke()`方法来处理。当然，还有很多其他Actuator的端点没有列在代码清单7-5里，这种省略完全是为了简化代码示例。

Actuator的配置端点能很方便地让你了解应用程序是如何配置的。能看到应用程序在运行时究竟发生了什么，这很有趣、很实用。度量端点能展示应用程序运行时内部状况的快照。

### 7.1.2 运行时度量

你到医生那里体检时，会做一系列检查来了解身体状况。有一些重要的项目永远不会变，比如血型。这类测试能让医生了解你身体的一贯情况。其他测试让医生掌握你接受检查时的身体状况。你的心律、血压和胆固醇水平有助于医生评估你的健康。这些指标都是临时的，很可能随时间发生变化，但它们同样是很有帮助的运行时指标。

与之类似，对运行时度量情况做一个快照，这对评估应用程序的健康情况很有帮助。Actuator提供了一系列端点，让你能在运行时快速检查应用程序。让我们来了解一下这些端点，从`/metrics`开始。

####1.查看应用程序的度量值

关于运行中的应用程序，有很多有趣而且有用的信息。举个例子，了解应用程序的内存情况（可用或空闲）有助于决定给JVM分配多少内存。对Web应用程序而言，不用查看Web服务器日志，如果请求失败或者是耗时太长，就可以大概知道内存的情况了。

运行中的应用程序有诸多计数器和度量器，/metrics端点提供了这些东西的快照。代码清单7-6是/metrics端点输出内容的示例。

__代码清单7-6 /metrics端点提供了很多有用的运行时数据__

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

如你所见，/metrics端点提供了很多信息，逐行查看这些度量值太麻烦。表7-2根据所提供信息的类型对它们做了个分类。

__表7-2 /metrics端点报告的度量值和计数器__

| 分类      | 前缀              | 报告内容                                            |
|----------|-------------------|---------------------------------------------------|
| 垃圾收集器 | `gc.*` | 已经发生过的垃圾收集次数，以及垃圾收集所耗费的时间，适用于标记-清理垃圾收集器和并行垃圾收集器（数据源自`java.lang.management.GarbageCollectorMXBean`） |
| 内存 | `mem.*` | 分配给应用程序的内存数量和空闲的内存数量（数据源自`java.lang.Runtime`） |
| 堆 | `heap.*` | 当前内存用量（数据源自`java.lang.management.MemoryUsage`） |
| 类加载器 | `classes.*` | JVM类加载器加载与卸载的类的数量（数据源自`java.lang.management.ClassLoadingMXBean`） |
| 系统 | `processors`、`uptime instance.uptime`、`systemload.average` | 系统信息，例如处理器数量（数据源自`java.lang.Runtime`）、运行时间（数据源自`java.lang.management.RuntimeMXBean`）、平均负载（数据源自`java.lang.management.OperatingSystemMXBean`） |
| 线程池 | `threads.*` | 线程、守护线程的数量，以及JVM启动后的线程数量峰值（数据源自`java.lang .management.ThreadMXBean`） |
| 数据源 | `datasource.*` | 数据源连接的数量（源自数据源的元数据，仅当Spring应用程序上下文里存在`DataSource` Bean的时候才会有这个信息） |
| Tomcat会话 | `httpsessions.*` | Tomcat的活跃会话数和最大会话数（数据源自嵌入式Tomcat的Bean，仅在使用嵌入式Tomcat服务器运行应用程序时才有这个信息） |
| HTTP | `counter.status.*`、 `gauge.response.*` | 多种应用程序服务HTTP请求的度量值与计数器 |

请注意，这里的一些度量值，比如数据源和Tomcat会话，仅在应用程序中运行特定组件时才有数据。你还可以注册自己的度量信息，7.4.3节里会提到这一点。

HTTP的计数器和度量值需要做一点说明。counter.status后的值是HTTP状态码，随后是所请求的路径。举个例子，`counter.status.200.metrics`表明/metrics端点返回200（OK）状态码的次数。

HTTP的度量信息在结构上也差不多，却在报告另一类信息。它们全部用gauge.response开头，表明这是HTTP响应的度量信息。前缀后是对应的路径。度量值是以毫秒为单位的时间，反映了最近处理该路径请求的耗时。举个例子，代码清单7-6里的`gauge.response.beans`说明上一次请求耗时169毫秒。

这里还有几个特殊的值需要注意。root路径指向的是根路径或`/`。star-star代表了那些Spring认为是静态资源的路径，包括图片、JavaScript和样式表，其中还包含了那些找不到的资源。这就是为什么你经常会看到`counter.status.404.star-star`，这是返回了HTTP 404（NOT FOUND）状态的请求数。

/metrics端点会返回所有的可用度量值，但你也可能只对某个值感兴趣。要获取单个值，请求时可以在URL后加上对应的键名。例如，要查看空闲内存大小，可以向/metrics/mem.free发一个`GET`请求：

```
$ curl localhost:8080/metrics/mem.free
144029
```

要知道，虽然响应里的`Content-Type`头设置为application/json;charset=UTF-8，但实际/metrics/{name}的结果是文本格式的。因此，如果需要的话，你也可以把它视为JSON来处理。

####2.追踪Web请求

尽管/metrics端点提供了一些针对Web请求的基本计数器和计时器，但那些度量值缺少详细信息。知道所处理请求的更多信息是很有帮助的，尤其是在调试时，所以就有了/trace这个端点。

/trace端点能报告所有Web请求的详细信息，包括请求方法、路径、时间戳以及请求和响应的头信息。代码清单7-7是/trace输出的一个片段，其中包含了整个请求跟踪项。

__代码清单7-7 /trace端点会记录下Web请求的细节__

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

正如`method`和`path`属性所示，你可以看到这个跟踪项是一个针对/metrics的请求。`timestamp`属性（以及响应中的`Date`头）告诉了你请求的处理时间。`headers`属性的内容是请求和响应中所携带的头信息。

虽然代码清单7-7里只显示了一条跟踪项，但/trace端点实际能显示最近100个请求的信息，包含对/trace自己的请求。它在内存里维护了一个跟踪库。稍后在7.4.4节里，你会看到如何创建一个自定义的跟踪库实现，以便将请求的跟踪持久化。

####3.导出线程活动

在确认应用程序运行情况时，除了跟踪请求，了解线程活动也会很有帮助。/dump端点会生成当前线程活动的快照。

完整的线程导出报告里会包含应用程序的每个线程。为了节省空间，代码清单7-8里只放了一个线程的内容片段。如你所见，其中包含很多线程的特定信息，还有线程相关的阻塞和锁状态。本例中，还有一个跟踪栈（stack trace），表明这是一个Tomcat容器线程。

__代码清单7-8 /dump端点提供了应用程序线程的快照__

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

####4.监控应用程序健康情况

如果你想知道自己的应用程序是否在运行，可以直接访问/health端点。在最简单的情况下，该端点会显示一个简单的JSON，内容如下：

```
{"status":"UP"}
```

`status`属性显示了应用程序在运行中。当然，它的确在运行，此处的响应无关紧要，任何输出都说明这个应用程序在运行。但`/health`端点可以输出的信息远远不止简单的`UP`状态。

/health端点输出的某些信息可能涉及内容，因此对未经授权的请求只能提供简单的健康状态。如果经过身份验证（比如你已经登录了），则可以提供更多信息。下面是阅读列表应用程序一些健康信息的示例：

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

除了基本的健康状态，可用的磁盘空间以及应用程序正在使用的数据库状态也可以看到。

/health端点所提供的所有信息都是由一个或多个健康指示器提供的。表7-3列出了Spring Boot自带的健康指示器。

__表7-3 Spring Boot自带的健康指示器__

| 健康指示器 | 键 | 报告内容 |
|----------|-----|---------|
|` ApplicationHealthIndicator` | `none` | 永远为`UP` |
| `DataSourceHealthIndicator` | `db` | 如果数据库能连上，则内容是`UP`和数据库类型；否则为`DOWN` |
| `DiskSpaceHealthIndicator` | `diskSpace` | 如果可用空间大于阈值，则内容为`UP`和可用磁盘空间；如果空间不足则为`DOWN` |
| `JmsHealthIndicator` | `jms` | 如果能连上消息代理，则内容为`UP`和JMS提供方的名称；否则为`DOWN` |
| `MailHealthIndicator` | `mail` | 如果能连上邮件服务器，则内容为`UP`和邮件服务器主机和端口；否则为`DOWN` |
| `MongoHealthIndicator` | `mongo` | 如果能连上MongoDB服务器，则内容为`UP`和MongoDB服务器版本；否则为`DOWN` |
| `RabbitHealthIndicator` | `rabbit` | 如果能连上RabbitMQ服务器，则内容为`UP`和版本号；否则为`DOWN` |
| `RedisHealthIndicator` | `redis` | 如果能连上服务器，则内容为`UP`和Redis服务器版本；否则为`DOWN` |
| `SolrHealthIndicator` | `solr` | 如果能连上Solr服务器，则内容为`UP`；否则为`DOWN` |

这些健康指示器会按需自动配置。举例来说，如果Classpath里有`javax.sql.DataSource`，则会自动配置`DataSourceHealthIndicator`。`ApplicationHealthIndicator`和`DiskSpaceHealthIndicator`则会一直配置着。

除了这些自带的健康指示器，你还会在7.4.5节里看到如何创建自定义健康指示器。

### 7.1.3 关闭应用程序

假设你要关闭运行中的应用程序。比方说，在微服务架构中，你有多个微服务应用的实例运行在云上，其中某个实例有问题了，你决定关闭该实例并让云服务提供商为你重启这个有问题的应用程序。在这个场景中，Actuator的/shutdown端点就很有用了。

为了关闭应用程序，你要往/shutdown发送一个`POST`请求。例如，可以用命令行工具`curl`来关闭应用程序：

```
$ curl -X POST http://localhost:8080/shutdown
```
很显然，关闭运行中的应用程序是件危险的事情，因此这个端点默认是关闭的。如果没有显式地开启这个功能，那么`POST`请求的结果是这样的：
```
{"message":"This endpoint is disabled"}
```  
要开启该端点，可以将`endpoints.shutdown.enabled`设置为`true`。举例来说，可以把如下内容加入application.yml，借此开启/shutdown端点：

```
endpoints:
  shutdown:
    enabled: true
```

打开/shutdown端点后，你要确保并非任何人都能关闭应用程序。这时应该保护/shutdown端点，只有经过授权的用户能关闭应用程序。在7.5节里你将看到如何保护Actuator端点。

### 7.1.4 获取应用信息

Spring Boot Actuator还有一个有用的端点。/info端点能展示各种你希望发布的应用信息。针对该端点的`GET`请求的默认响应是这样的：

```
{}
```
很显然，一个空的JSON对象没什么用。但你可以通过配置带有`info`前缀的属性向/info端点的响应添加内容。例如，你希望在响应中添加联系邮箱。可以在application.yml里设置名为`info.contactEmail`的属性：

```
info:
  contactEmail: support@myreadinglist.com
```

现在再访问/info端点，就能得到如下响应：

```
{
  "contactEmail":"support@myreadinglist.com"
}
```

这里的属性也可以是嵌套的。例如，假设你希望提供联系邮箱和电话。在application.yml里可以配置如下属性：

```
info:
  contact:
    email: support@myreadinglist.com
    phone: 1-888-555-1971
```

/info端点返回的JSON会包含一个`contact`属性，其中有`email`和`phone`属性：

```
{
  "contact":{
    "email":"support@myreadinglist.com",
    "phone":"1-888-555-1971"
  }
}
```

向/info端点添加属性只是定制Actuator行为的众多方式之一。稍后在7.4节里，我们还会看到其他配置与扩展Actuator的方式。但现在，先让我们来看看如何保护Actuator的端点。

## 7.2 连接Actuator的远程shell

Actuator通过REST端点提供了不少非常有用的信息。另一个深入运行中应用程序内部的方式是使用远程shell。Spring Boot集成了CRaSH，一种能嵌入任意Java应用程序的shell。Spring Boot还扩展了CRaSH，添加了不少Spring Boot特有的命令，提供了与Actuator端点类似的功能。

要使用远程shell，只需加入远程shell的起步依赖即可。你需要这样的Gradle依赖：
```
compile("org.springframework.boot:spring-boot-starter-remote-shell")
```
如果用Maven构建项目，你需要在pom.xml文件里添加如下依赖：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-remote-shell</artifactId>
</dependency>
```

如果要用Spring Boot CLI来运行你所开发的应用程序，则需要如下`@Grab`注解：

```
@Grab("spring-boot-starter-remote-shell")
```

添加了远程shell依赖后，就可以构建并运行应用程序了。在启动的时候，可以看到要写进日志的一行密码。这行密码所在的行大概是这样的：

```
Using default security password: efe30c70-5bf0-43b1-9d50-c7a02dda7d79
```

与这个密码搭配使用的用户名是user。密码本身是随机生成的，每次运行应用程序时都会有所变化。

现在你可以通过SSH工具连接shell了，它监听的端口号是2000。如果你用的是Unix的`ssh`命令，那么它看起来大概是这样的：

> P142 代码

太棒了！你已经连上shell了。现在应该做什么？

远程shell提供了24个可以在运行应用程序上下文中执行的命令，其中大部分都是CRaSH自带的。但Spring Boot也添加了一些。表7-4列出了这些Spring Boot特有的命令。

__表7-4 Spring Boot提供的CRaSH shell命令__

| 命令 | 描述 |
|-----|-----|
| `autoconfig` | 生成自动配置说明报告，和/autoconfig端点输出的内容类似，只是把JSON换成了纯文本 |
| `beans` | 列出Spring应用程序上下文里的Bean，与/beans端点输出的内容类似 |
| `endpoint` | 调用Actuator端点|
| `metrics` | 显示Spring Boot的度量信息，与/metrics端点类似，但显示的是实时更新的数据 |

让我们看看如何使用这些Spring Boot添加的shell命令。

### 7.2.1 查看`autoconfig`报告

`autoconfig`命令生成了一个与Actuatord的/autoconfig端点类似的报告。图7-1是`autoconfig`命令输出的内容截屏片段。

>P143图

__图7-1 `autoconfig`命令的输出__

如你所见，结果分为两组——匹配和不匹配，和/autoconfig端点的结果一样。实际上，唯一的显著区别在于，`autoconfig`命令输出的是文本，而/autoconfig端点输出的是JSON，其他都一样。

我不打算去讲CRaSH自己提供的shell命令，但你可能想把`autoconfig`命令的输出和CRaSH的`less`命令用管道串起来：

```
> autoconfig | less
```

`less`命令和Unix shell里的同名命令很相似，能让你穿梭于文件中。`autoconfig`的输出很长，但`less`命令会让它更容易读取和查阅。

### 7.2.2 列出应用程序的Bean

`autoconfig`shell命令的输出和/autoconfig端点的输出类似，但也有不同。对比之下，你会发现`beans`命令的输出和/beans端点的输出一样。截屏如图7-2所示。

>P144图

__图7-2 `beans`命令的输出__

和/beans端点一样，`beans`命令会以JSON格式列出Spring应用程序上下文里所有的Bean，包括所依赖的Bean。

### 7.2.3 查看应用程序的度量信息

`metrics`shell命令会输出与Actuator的/metrics端点一样的信息。/metrics端点以JSON格式输出当前度量信息的快照，而`metrics`命令则会接管shell，以实时仪表盘显示结果。图7-3就是`metrics`命令的仪表盘。

>P144上图

__图7-3 `metrics`命令的仪表盘__

`metrics`命令的实时仪表盘很难在书里以静态图片演示。但你可以试着想象一下，内存、堆、线程在不断消耗和释放，随着类的加载，仪表盘里显示的数量也会随之变化，以反映当前值。

一旦看完了`metrics`命令提供的度量信息，按Ctrl+C就能回到shell了。

### 7.2.4 调用Actuator端点

你现在应该已经意识到了，并非所有的Actuator端点都有对应的shell命令。这是否意味着shell不能完全代替Actuator端点呢？是否仍要直接查询这些端点来获取Actuator提供的内部信息呢？虽然shell没能完全匹配上这些端点，但`endpoint`命令可以让你在shell里调用Actuator的端点。

首先，你要知道自己想调用哪个端点。在shell提示符中键入`endpoint list`就能获得端点的列表，如图7-4所示。请注意，列表中的端点用的是它们的Bean名称，而非URL路径。

>P 144下图

__图7-4 获得端点列表__

如果想在shell里调用其中某个端点，你可以使用`endpoint invoke`命令，传入不带Endpoint后缀的Bean名称。举例来说，要调用健康检查端点，可以在shell提示符里键入`endpoint invoke health`，如图7-5所示。

>P146上图

__图7-5 调用健康检查端点__

请注意，这些端点返回的信息都是原始格式的，即未格式化过的JSON文档。虽然在shell里调用Actuator的端点不错，但输出结果很难阅读。就这个问题，自带的功能帮不上忙。但如果爱折腾，你也可以创建一个自定义的CRaSH shell命令，通过管道接受未格式化的JSON，然后美化输出。你总是可以剪切黏贴`endpoint`命令的输出，将其放入你喜欢的工具进行阅读或格式化。

## 7.3 通过JMX监控应用程序

除了REST端点和远程shell，Actuator还把它的端点以MBean的方式发布了出来，可以通过JMX来查看和管理。JMX是种不错的Spring Boot应用程序管理方式，如果你已在用JMX管理应用程序中的其他MBean，则尤其如此。

Actuator的端点都发布在org.springframework.boot域下。比如，你想要查看应用程序的请求映射关系，那么可以看一下图7-6（通过JConsole查看请求映射端点）。

>P146下图

__图7-6 通过JConsole查看请求映射端点__

如你所见，在`requestMappingEndpoint`下可以找到请求映射端点，位于org.springframework.boot域中的`Endpoint`下。`Data`属性中包含了该端点所要输出的JSON内容。

和其他MBean一样，端点MBean有可供调用的操作。大部分端点MBean只有访问操作，返回其中的某个属性，但关闭端点提供了一些有趣（同时具有毁灭性）的操作，如图7-7所示。

>P147上图

__图7-7 调用该端点的关闭按钮__

如果你想要关闭应用程序（或者喜欢冒险的生活），那么关闭应用的端点正合你意。如图7-7所示，这个界面就等你点击shutdown按钮调用该端点。请小心，这里没有“后悔药”，也没有“你确定吗？”之类的提示。

接下来你会看图7-8。

>P147下图

__图7-8 应用程序立马关闭__

在那以后，你的应用程序就关闭了。应用已经关闭，自然就没办法发布其他用来重启它的MBean操作。你必须重启，和一开始的启动方式一样。

## 7.4 定制Actuator

虽然Actuator提供了很多运行中Spring Boot应用程序的内部工作细节，但难免和你的需求有所偏差。也许你并不需要它提供的所有功能，想要关闭一些也说不定。或者，你需要对Actuator稍作扩展，增加一些自定义的度量信息，以满足你对应用程序的需求。

实际上，Actuator有多种定制方式，包括以下五项

* 重命名端点。
* 启用和禁用端点。
* 自定义度量信息。
* 创建自定义仓库来存储跟踪数据。
* 插入自定义的健康指示器。

接下来，我们会了解如何定制Actuator，让它满足我们的需要。先来看一个最简单的定制：重命名Actuator端点。

### 7.4.1 修改端点ID

每个Actuator端点都有一个ID用来决定端点的路径，比方说，/beans端点的默认ID就是`beans`。

如果端点的路径是由ID决定的，那么可以通过修改ID来改变端点的路径。你要做的就是设置一个属性，属性名是`endpoints.endpoint-id.id`。

我们用/shutdown端点来做个演示，它会响应发往/shutdown的`POST`请求。假设你想让它处理发往/kill的`POST`请求，可以通过如下YAML为/shutdown赋予一个新的ID，也就是新的路径：

```
endpoints:
  shutdown:
    id: kill
```

重命名端点、修改其路径的理由很多。最明显的理由就是，端点的命名要和团队的术语保持一致。你也可能想重命名端点，让那些熟悉默认名称的人找不到它，借此增加一些安全感。

遗憾的是，重命名端点并不能真的起到保护作用，顶多是让黑客慢点找到它们。我们会在7.5节看到如何保护这些Actuator端点。现在先让我们来看看如何禁用某个（或全部）不希望别人访问的端点。

### 7.4.2 启用和禁用端点

虽然Actuator的端点都很有用，但你不一定需要全部这些端点。默认情况下，所有端点（除了/shutdown）都启用。我们已经看过如何设置`endpoints.shutdown.enabled`为`true`，以此开启/shutdown端点（详见7.1.1节）。用同样的方式，你可以禁用其他的端点，将`endpoints.endpoint-id.enabled `设置为`false`。

例如，要禁用/metrics端点，你要做的就是将`endpoints.metrics.enabled`属性设置为`false`。在application.yml里做如下设置：
```
endpoints:
  metrics:
    enabled: false
```
如果你只想打开一两个端点，那就先禁用全部端点，然后启用那几个你要的，这样更方便。例如，考虑如下application.yml片段：
```
endpoints:
  enabled: false
  metrics:
    enabled: true
```
正如以上片段所示，`endpoints.enabled`设置为`false`就能禁用Actuator的全部端点，然后将`endpoints.metrics.enabled`设置为`true`重新启用/metrics端点。

### 7.4.3 添加自定义度量信息

在7.1.2节中，你看到了如何从/metrics端点获得运行中应用程序的内部度量信息，包括内存、垃圾回收和线程信息。这些都是非常有用且信息量很大的度量值，但你可能还想定义自己的度量，用来捕获应用程序中的特定信息。

比方说，我们想要知道用户往阅读列表里保存了多少次图书，最简单的方法就是在每次调用`ReadingListController`的`addToReadingList()`方法时增加计数器值。计数器很容易实现，但这个不断变化的总计值如何同/metrics端点发布的度量信息一起发布出来呢？

再假设我们想要获得最后保存图书的时间戳。时间戳可以通过调用`System.currentTimeMillis()`来获取，但如何在/metrics端点里报告该时间戳呢？

实际上，自动配置允许Actuator创建`CounterService`的实例，并将其注册为Spring的应用程序上下文中的Bean。`CounterService`这个接口里定义了三个方法，分别用来增加、减少或重置特定名称的度量值，代码如下：

```
package org.springframework.boot.actuate.metrics;

public interface CounterService {
  void increment(String metricName);
  void decrement(String metricName);
  void reset(String metricName);
}
```
Actuator的自动配置还会配置一个`GaugeService`类型的Bean。该接口与`CounterService`类似，能将某个值记录到特定名称的度量值里。`GaugeService`看起来是这样的：

```
package org.springframework.boot.actuate.metrics;

public interface GaugeService {
  void submit(String metricName, double value);
}
```

你无需实现这些接口。Spring Boot已经提供了两者的实现。我们所要做的就是把它们的实例注入所需的Bean，在适当的时候调用其中的方法，更新想要的度量值。

针对上文提到的需求，我们需要把`CounterService`和`GaugeService` Bean注入`ReadingListController`，然后在`addToReadingList()`方法里调用其中的方法。代码清单7-9是`ReadingListController`里的相关变动：

__代码清单7-9 使用注入的`CounterService`和`GaugeService`__

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
>文字

>Inject the counter and gauge services：注入`CounterService`和`GaugeService`

>Increment “books.saved”：增加`books.saved`的值

>Record “books.last.saved”：记录`books.last.saved`的值

修改后的`ReadingListController`使用了自动织入机制，通过控制器的构造方法注入`CounterService`和`GaugeService`，随后把它们保存在实例变量里。此后，`addToReadingList()`方法每次处理请求时都会调用`counterService.increment("books.saved")`和`gaugeService.submit("books.last.saved")`来调整度量值。

尽管`CounterService`和`GaugeService`用起来很简单，但还是有一些度量值很难通过增加计数器或记录指标值来捕获。对于那些情况，我们可以实现`PublicMetrics`接口，提供自己需要的度量信息。该接口定义了一个`metrics()`方法，返回一个`Metric`对象的集合：

```
package org.springframework.boot.actuate.endpoint;

public interface PublicMetrics {
  Collection<Metric<?>> metrics();
}
```

为了解`PublicMetrics`的使用方法，这里假设我们想报告一些源自Spring应用程序上下文的度量值——应用程序上下文启动的时间、Bean及Bean定义的数量，这些都包含进来会很有意思。顺便再报告一下添加了`@Controller`注解的Bean的数量。代码清单7-10给出了相应`PublicMetrics`实现的代码。

__代码清单7-10 发布自定义度量信息__

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
> 文字

>Record startup date：记录启动时间

>Record bean definition count：记录Bean定义数量

>Record bean count：记录Bean数量

>Record controller bean count：记录控制器类型的Bean数量

Actuator会调用`metrics()`方法，收集`ApplicationContextMetrics`提供的度量信息。该方法调用了所注入的`ApplicationContext`上的方法，获取我们想要报告为度量的数量。每个度量值都会创建一个`Metrics`实例，指定度量的名称和值，将其加入要返回的列表。

创建`ApplicationContextMetrics`，并在`ReadingListController`里使用`CounterService`和`GaugeService`之后，我们可以在/metrics端点的响应中找到如下条目：

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

当然，这些度量的实际值会根据添加了多少书、何时启动应用程序及何时保存最后一本书而发生变化。在这个例子里，你一定会好奇为什么`spring.controllers`是2。因为这里算上了`ReadingListController`以及Spring Boot提供的`BasicErrorController`。

### 7.4.4 创建自定义跟踪仓库

默认情况下，/trace端点报告的跟踪信息都存储在内存仓库里，100个条目封顶。一旦仓库满了，就开始移除老的条目，给新的条目腾出空间。在开发阶段这没什么问题，但在生产环境中，大流量会造成跟踪信息还没来得及看就被丢弃。

为了避免这个问题，你可以声明自己的`InMemoryTraceRepository` Bean，将它的容量调整至100以上。如下配置类可以将容量调整至1000个条目：

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

仓库容量翻了十倍，跟踪信息的保存时间应该会更久。不过，繁忙到一定程度，应用程序还是可能在你查看这些信息前将其丢弃。这是一个内存存储的仓库，还要避免容量增长太多，影响应用程序的内存使用。

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

如你所见，`TraceRepository`只要求我们实现两个方法：一个方法查找所有存储的`Trace`对象，另一个保存了一个`Trace`，包含跟踪信息的`Map`对象。

作为演示，假设我们创建了一个使用MongoDB数据库存储跟踪信息的`TraceRepository`实例。代码清单7-11演示了如何实现这个`TraceRepository`。

__代码清单7-11 往MongoDB保存跟踪数据__

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
> 文字

> Inject MongoOperations：注入`MongoOperations`

> Fetch all trace entries：获取所有跟踪条目

> Save a trace entry：保存一个跟踪条目

`findAll()`方法很直白，用注入的`MongoOperations`来查找全部`Trace`对象。`add()`方法稍微有趣一点，用当前时间和含有跟踪信息的`Map`创建了一个`Trace`对象，然后通过`MongoOperations.save()`将其保存下来。唯一的问题是，`MongoOperations`是哪里来的？

为了使用`MongoTraceRepository`，我们需要保证Spring应用程序上下文里先有一个`MongoOperations` Bean。得益于Spring Boot的起步依赖和自动配置，做到这一点只需添加MongoDB起步依赖即可。你需要如下Gradle依赖：

```
compile("org.springframework.boot:spring-boot-starter-data-mongodb")
```

如果你用的是Maven，则需要如下依赖：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

添加了这个起步依赖后，Spring Data MongoDB和所依赖的库会添加到应用程序的Classpath里。Spring Boot会自动配置所需的Bean，以便使用MongoDB数据库。这些Bean里就包括`MongoOperations`。另外，你需要确保和`MongoOperations`通讯的MongoDB服务器正常运行。

### 7.4.5 插入自定义健康指示器

如前文所述，Actuator自带了很多健康指示器，能满足常见需求，比如报告应用程序使用的数据库和消息代理的健康情况。但如果你的应用程序需要和一些没有健康指示器的系统交互，那该怎么办呢？

我们的阅读列表里有指向Amazon的图书链接，可以报告一下Amazon是否可以访问。当然，Amazon不太可能宕机，但不怕一万就怕万一，所以让我们为Amazon创建一个健康指示器吧。代码清单7-12演示了相关`HealthIndicator`的实现。

__代码清单7-12 自定义一个Amazon健康指示器__

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
>文字

>Send request to Amazon：向Amazon发送请求

>Report “down” health：报告`DOWN`状态

`AmazonHealth`类并没有什么花哨的地方。`health()`方法只是使用Spring的`RestTemplate`向Amazon首页发起了一个`GET`请求。如果请求成功，则返回一个表明Amazon状态为`UP`的`Health`对象。如果请求发生异常，则`health()`返回一个标明Amazon状态为`DOWN`的`Health`对象。

下面是/health端点响应的一个片段。这里可以看出，如果Amazon不可访问，你会看到什么。

```
{
  "amazonHealth": {
    "status": "DOWN"
  },
  ...
}
```

你不会相信我等Amazon宕机等了多久，就为了能看到上面的结果！***{![实际上我并没有等太久。我只是把电脑的网络断开了。没有网就没有Amazon。]}***

除了简单的状态之外，如果你还想向健康记录里添加其他附加信息，可以调用`Health`构造器的`withDetail()`方法。例如，要添加异常消息，将其作为健康记录的`reason`字段，可以让`catch`块返回这样一个`Health`对象：

```
return Health.down().withDetail("reason", e.getMessage()).build();
```

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

如果你有很多附加信息，可以多次调用`withDetail()`方法，每次设置一个想要放入健康记录的附加字段。

## 7.5 保护Actuator端点

很多Actuator端点发布的信息都可能涉及敏感数据，还有一些端点，（比如/shutdown）非常危险，可以用来关闭应用程序。因此，保护这些端点尤为重要，能访问它们的只能是那些经过授权的客户端。

实际上，Actuator的端点保护可以用和其他URL路径一样的方式——使用Spring Security。在Spring Boot应用程序中，这意味着将Security起步依赖作为构建依赖加入，然后让安全相关的自动配置来保护应用程序，其中当然也包括了Actuator端点。

在第3章，我们看到了默认安全自动配置如何把所有URL路径保护起来，要求HTTP基本身份验证，用户名是user，密码在启动时随机生成并写到日志文件里去。这不是我们所希望的Actuator保护方式。

我们已经添加了一些自定义安全配置，仅限带有READER权限的授权用访问根URL路径（`/`）。要保护Actuator的端点，我们需要对`SecurityConfig.java`的`configure()`方法做些修改。

举例来说，你想要保护/shutdown端点，仅允许拥有ADMIN权限的用户访问，代码清单7-13就是新的`configure()`方法。

__代码清单7-13 保护/shutdown端点__

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
>文字

>Require ADMIN access：要求有ADMIN权限

现在要访问/shutdown端点，必须用一个带ADMIN权限的用户来做身份验证。

然而，第3章里的自定义`UserDetailsService`只对通过`ReaderRepository`加载的用户赋予READER权限。因此，你需要创建一个更聪明的`UserDetailsService`实现，对某些用户赋予ADMIN权限。你可以配置一个额外的身份验证实现，比如代码清单7-14里的内存实现。

__代码清单7-14 添加一个内存里的admin用户__

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
>文字

>Reader authentication：Reader身份验证

>Admin authentication：Admin身份验证

新加的内存身份验证中，用户名定义为admin，密码为s3cr3t，同时被授予ADMIN和READER权限。

现在，除了那些拥有ADMIN权限的用户，谁都无法访问/shutdown端点。但Actuator的其他端点呢？假设你只想让ADMIN的用户访问它们（像/shutdown一样），可以在调用`antMatchers()`时列出这些URL。例如，要保护/metrics、/confiprops和/shutdown，可以像这样调用`antMatchers()`：

```
.antMatchers("/shutdown", "/metrics", "/configprops")
                          .access("hasRole('ADMIN')")
```

虽然这么做能奏效，但也只适用于少数Actuator端点的保护。如果要保护全部Actuator端点，这种做法就不太方便了。

比起在调用`antMatchers()`方法时显式地列出所有的Actuator端点，用通配符在一个简单的Ant风格表达式里匹配全部的Actuator端点更容易。但是，这么做也小有点挑战，因为不同的端点路径之间没有什么共同点，我们也不能在`/**`上运用ADMIN权限。这样一来，除了根路径（`/`）之外，什么要有ADMIN权限。

为此，可以通过`management.context-path`属性设置端点的上下文路径。默认情况下，这个属性是空的，所以Actuator的端点路径都是相对于根路径的。在application.yaml里增加如下内容，可以让这些端点都带上`/mgmt`前缀。

```
management:
  context-path: /mgmt
```

你也可以在application.properties里做类似的事情：

```
management.context-path=/mgmt
```

将`management.context-path`设置为/mgmt后，所有的Actuator端点都会与/mgmt路径相关。例如，/metrics端点的URL会变为/mgmt/metrics。

有了这个新的路径，我们就有了公共的前缀，在为Actuator端点赋予ADMIN权限限制时就能借助这个公共前缀：

```
.antMatchers("/mgmt/**").access("hasRole('ADMIN')")
```

现在所有以/mgmt开头的请求（包含了所有的Actuator端点），都只让授予了ADMIN权限的认证用户访问。

## 7.6 小结

想弄清楚运行的应用程序里正在发生什么，这是件很困难的事。Spring Boot的Actuator为你打开了一扇大门，深入Spring Boot应用程序的内部细节。它发布的组件、度量和指标能帮你理解应用程序的运作情况。

在本章，我们先了解了Actuator的Web端点——通过HTTP发布运行时细节信息的REST端点。这些端点的功能包括查看Spring应用程序上下文里所有的Bean、查看自动配置决策、查看Spring MVC映射、查看线程活动、查看应用程序健康信息，还有多种度量、指标和计数器。

除了Web端点，Actuator还提供了另外两种获取它所提供信息的途径。远程shell让你能在shell里安全地连上应用程序，发起指令，获得与Actuator端点相同的数据。与此同时，所有的Actuator端点也都发布成了MBean，可以通过JMX客户端进行监控和管理。

随后我们还了解了如何定制Actuator，包括如何通过端点的ID来修改Actuator端点的路径，如何启用和禁用端点，诸如此类。我们还插入了一些定制的度量信息，创建了定制的跟踪信息仓库，替换了默认的内存跟踪仓库。

最后，我们学习了如何保护Actuator的端点，只让经过授权的用户访问它们。

接下来，在第8章里，我们将看到如何让应用程序从编码阶段过渡到生产阶段，了解Spring Boot如何协助我们在多种不同的平台上进行部署，包括传统的应用容器和云平台。
