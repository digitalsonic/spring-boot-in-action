# Taking a peek inside with the Actuator

__This chapter covers__
__本章内容涉及__

* Actuator web endpoints
* Adjusting the Actuator
* Shelling into a running application
* Securing the Actuator

Have you ever tried to guess what’s inside a wrapped gift? You shake it, weigh it, and measure it. And you might even have a solid idea as to what’s inside. But until you open it up, there’s no way of knowing for sure.

A running application is kind of like a wrapped gift. You can poke at it and make reasonable guesses as to what’s going on under the covers. But how can you know for sure? If only there were some way that you could peek inside a running application, see how it’s behaving, check on its health, and maybe even trigger operations that influence how it runs?

In this chapter, we’re going to explore Spring Boot’s Actuator. The Actuator offers production-ready features such as monitoring and metrics to Spring Boot applications. The Actuator’s features are provided by way of several REST endpoints, a remote shell, and Java Management Extensions (JMX). We’ll start by looking at the Actuator’s REST endpoints, which offer the most complete and well-known way of working with the Actuator.

## 7.1 Exploring the Actuator’s endpoints

The key feature of Spring Boot’s Actuator is that it provides several web endpoints in your application through which you can view the internals of your running application. Through the Actuator, you can find out how beans are wired together in the Spring application context, determine what environment properties are available to your application, get a snapshot of runtime metrics, and more.

The Actuator offers a baker’s dozen of endpoints, as described in table 7.1.

__Table 7.1 Actuator endpoints__

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

To enable the Actuator endpoints, all you must do is add the Actuator starter to your build. In a Gradle build specification, that dependency looks like this:

```
compile 'org.springframework.boot:spring-boot-starter-actuator'
```

For a Maven build, the required dependency is as follows:

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Or, if you’re using the Spring Boot CLI, the following @Grab should do the trick:

```
@Grab('spring-boot-starter-actuator')
```

No matter which technique you use to add the Actuator to your build, auto-configuration will kick in when the application is running and you enable the Actuator.

The endpoints in table 7.1 can be organized into three distinct categories: configuration endpoints, metrics endpoints, and miscellaneous endpoints. Let’s take a look at each of these endpoints, starting with the endpoints that provide insight into the configuration of your application.

### 7.1.1 Viewing configuration details

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