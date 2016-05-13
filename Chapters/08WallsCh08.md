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
Spring Boot应用程序有多种构建和运行的方式，其中一些你已经用过了：

* Running the application in the IDE (either Spring ToolSuite or IntelliJ IDEA)
* Running from the command line using the Maven spring-boot:run goal or Gradle bootRun task
* Using Maven or Gradle to produce an executable JAR file that can be run at the command line
* Using the Spring Boot CLI to run Groovy scripts at the command line
* Using the Spring Boot CLI to produce an executable JAR file that can be run at the command line
* 在IDE中运行应用程序（Spring ToolSuite或IntelliJ IDEA）
* 使用Maven的`spring-boot:run`或Gradle的`bootRun`在命令行里运行
* 使用Maven或Gradle来生成可运行的JAR文件，随后在命令行中运行
* 使用Spring Boot CLI在命令行中运行Groovy脚本
* 使用Spring Boot CLI来生成可运行的JAR文件，随后在命令行中运行

Any of these choices is suitable for running the application while you’re still developing it. But what about when you’re ready to deploy the application into a production or other non-development environment?  
这些选项中的每一种都适合用来运行正在开发的应用程序。但在要将应用程序部署到生产环境或其他非开发环境中时又该怎么办呢？

Although none of the choices listed seems fitting for deploying an application beyond development, the truth is that all but one of them is a valid choice. Running an application within the IDE is certainly ill-suited for a production deployment. Executable JAR files and the Spring Boot CLI, however, are still on the table and are great choices when deploying to a cloud environment.  
虽然这些选项里没有一个看起来能用在部署非开发环境，但事实上，它们之中除了在IDE中运行之外的那些选项都是适用于生产环境的。可运行的JAR文件和Spring Boot CLI还可用于将应用程序部署到云环境里。

That said, you’re probably wondering how to deploy a Spring Boot application to a more traditional application server environment such as Tomcat, WebSphere, or WebLogic. In those cases, executable JAR files and Groovy source code won’t work. For application server deployment, you’ll need your application wrapped up in a WAR file.  
也许你很想知道如何把Spring Boot应用程序部署到一个更加传统的应用服务器环境里，比如Tomcat、WebSphere或WebLogic。在这些情况中，可执行JAR文件和Groovy代码是不适用的。针对应用服务器的部署，需要将你的应用程序打包成一个WAR文件。

As it turns out, Spring Boot applications can be packaged for deployment in several ways, as described in table 8.1.  
实际上，Spring Boot应用程序可以用多种方式来打包，详见表8.1。

__Table 8.1 Spring Boot deployment choices__  
__表8.1 Spring Boot部署选项__

| Deployment artifact | Produced by                       | Target environment                                                                                           |
|---------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| Raw Groovy source   | Written by hand                   | Cloud Foundry and container deployment, such as with Docker                                                  |
| Executable JAR      | Maven, Gradle, or Spring Boot CLI | Cloud environments, including Cloud Foundry and Heroku, as well as container deployment, such as with Docker |
| WAR                 | Maven or Gradle                   | Java application servers or cloud environments such as Cloud Foundry                                         |

| 部署产物 | 产生方式 | 目标环境 |
|---------|---------|----------|
| Groovy源码 | 手写 | Cloud Foundry及容器部署，比如Docker |
| 可执行JAR | Maven、Gradle或Spring Boot CLI | 云环境，包括Cloud Foundry和Heroku，还有容器部署，比如Docker |
| WAR | Maven或Gradle | Java应用服务器或云环境，比如Cloud Foundry |

As you can see in table 8.1, your target environment will need to be a factor in your choice. If you’re deploying to a Tomcat server running in your own data center, then the choice of a WAR file has been made for you. On the other hand, if you’ll be deploying to Cloud Foundry, you’re welcome to choose any of the deployment options shown.  
如你所见，在做最终选择时需要考虑你的目标环境。如果要将应用程序部署到自己数据中心的Tomcat服务器上，WAR文件就是你的选择。另一方面，如果要部署到Cloud Foundry，可以使用表里列出的各种选项。

In this chapter, we’re going to focus our attention on the following options:  
在本章中，我们将关注以下这些选项：

* Deploying a WAR file to a Java application server
* Deploying an executable JAR file to Cloud Foundry
* Deploying an executable JAR file to Heroku (where the build is performed by Heroku)
* 向Java应用服务器里部署WAR文件
* 向Cloud Foundry里部署可执行JAR文件
* 向Heroku（构建过程是由Heroku执行的）里部署可执行JAR文件

As we explore these scenarios, we’re also going to have to deal with the fact that we’ve been using an embedded H2 database as we’ve developed the application, and we’ll look at ways to replace it with a production-ready database.  
这这些场景中，我们还要处理一个问题，在开发应用程序时我们使用了嵌入式的H2数据库，现在得把它替换成生产环境所需的数据库了。

To get started, let’s take a look at how we can build our reading-list application into a WAR file that can be deployed to a Java application server such as Tomcat, WebSphere, or WebLogic.  
首先，让我们看看如何将阅读列表应用程序构建为WAR文件，这样才能把它部署到Java应用服务器里，比如Tomcat、WebSphere或WebLogic。

## 8.2 Deploying to an application server

Thus far, every time we’ve run the reading-list application, the web application has been served from a Tomcat server embedded in the application. Compared to a conventional Java web application, the tables were turned. The application has not been deployed in Tomcat; rather, Tomcat has been deployed in the application.

Thanks in large part to Spring Boot auto-configuration, we’ve not been required to create a web.xml file or servlet initializer class to declare Spring’s DispatcherServlet for Spring MVC. But if we’re going to deploy the application to a Java application server, we’re going to need to build a WAR file. And so that the application server will know how to run the application, we’ll also need to include a servlet initializer in that WAR file.

### 8.2.1 Building a WAR file

As it turns out, building a WAR file isn’t that difficult. If you’re using Gradle to build the application, you simply must apply the “war” plugin:

```
apply plugin: 'war'
```

Then, replace the existing jar configuration with the following war configuration in build.gradle:

```
war {
  baseName = 'readinglist'
  version = '0.0.1-SNAPSHOT'
}
```

The only difference between this war configuration and the previous jar configuration is the change of the letter j to w.

If you’re using Maven to build the project, then it’s even easier to get a WAR file. All you need to do is change the <packaging> element’s value from jar to war.

```
<packaging>war</packaging>
```

Those are the only changes required to produce a WAR file. But that WAR file will be useless unless it includes a web.xml file or a servlet initializer to enable Spring MVC’s DispatcherServlet.

Spring Boot can help here. It provides SpringBootServletInitializer, a special Spring Boot-aware implementation of Spring’s WebApplicationInitializer. Aside from configuring Spring’s DispatcherServlet, SpringBootServletInitializer also looks for any beans in the Spring application context that are of type Filter, Servlet, or ServletContextInitializer and binds them to the servlet container.

To use SpringBootServletInitializer, simply create a subclass and override the configure() method to specify the Spring configuration class. Listing 8.1 shows ReadingListServletInitializer, a subclass of SpringBootServletInitializer that we’ll use for the reading-list application.

__Listing 8.1 Extending SpringBootServletInitializer for the reading-list application__

```
package readinglist;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.context.web.SpringBootServletInitializer;

public class ReadingListServletInitializer
       extends SpringBootServletInitializer {

  @Override
  protected SpringApplicationBuilder configure(
                                    SpringApplicationBuilder builder) {
    return builder.sources(Application.class);
  }

}
```

Specify Spring configuration

As you can see, the configure() method is given a SpringApplicationBuilder as a parameter and returns it as a result. In between, it calls the sources() method to register any Spring configuration classes. In this case, it only registers the Application class, which, as you’ll recall, served dual purpose as both a bootstrap class (with a main() method) and a Spring configuration class.

Even though the reading-list application has other Spring configuration classes, it’s not necessary to register them all with the sources() method. The Application class is annotated with @SpringBootApplication, which implicitly enables componentscanning. Component-scanning will discover and pull in any other configuration classes that it finds.

Now we’re ready to build the application. If you’re using Gradle to build the project, simply invoke the build task:

```
$ gradle build
```

Assuming no problems, the build will produce a file named readinglist-0.0.1-SNAPSHOT. war in build/libs.

For a Maven-based build, use the package goal:

```
$ mvn package
```

After a successful Maven build, the WAR file will be found in the “target” directory.

All that’s left is to deploy the application. The deployment procedure varies across application servers, so consult the documentation for your application server’s specific deployment procedure.

For Tomcat, you can deploy an application by copying the WAR file into Tomcat’s webapps directory. If Tomcat is running (or once it starts up if it isn’t currently running), it will detect the presence of the WAR file, expand it, and install it.

Assuming that you didn’t rename the WAR file before deploying it, the servlet context path will be the same as the base name of the WAR file, or /readinglist-0.0.1-SNAPSHOT in the case of the reading-list application. Point your browser at http://server:_port_/readinglist-0.0.1-SNAPSHOT to kick the tires on the app.

One other thing worth noting: even though we’re building a WAR file, it may still be possible to run it without deploying to an application server. Assuming you don’t remove the main() method from Application, the WAR file produced by the build can also be run as if it were an executable JAR file:

```
$ java -jar readinglist-0.0.1-SNAPSHOT.war
```

In effect, you get two deployment options out of a single deployment artifact!

At this point, the application should be up and running in Tomcat. But it’s still using the embedded H2 database. An embedded database was handy while developing the application, but it’s not a great choice in production. Let’s see how to wire in a different data source when deploying to production.

### 8.2.2 Creating a production profile

Thanks to auto-configuration, we have a DataSource bean that references an embedded H2 database. More specifically, the DataSource bean is a data source pool, typically org.apache.tomcat.jdbc.pool.DataSource. Therefore, it may seem obvious that in order to use some database other than the embedded H2 database, we simply need to declare our own DataSource bean, overriding the auto-configured DataSource, to reference a production database of our choosing.

For example, suppose that we wanted to work with a PostgreSQL database running on localhost with the name “readingList”. The following @Bean method would declare our DataSource bean:

```
@Bean
@Profile("production")
public DataSource dataSource() {
  DataSource ds = new DataSource();
  ds.setDriverClassName("org.postgresql.Driver");
  ds.setUrl("jdbc:postgresql://localhost:5432/readinglist");
  ds.setUsername("habuma");
  ds.setPassword("password");
  return ds;
}
```

Here the DataSource type is Tomcat’s org.apache.tomcat.jdbc.pool.DataSource, not to be confused with javax.sql.DataSource, which it ultimately implements. The details required to connect to the database (including the JDBC driver class name, the database URL, and the database credentials) are given to the DataSource instance. With this bean declared, the default auto-configured DataSource bean will be passed over.

The key thing to notice about this @Bean method is that it is also annotated with @Profile to specify that it should only be created if the “production” profile is active. Because of this, we can still use the embedded H2 database while developing the application, but use the PostgreSQL database in production by activating the profile.

Although that should do the trick, there’s a better way to configure the database details without explicitly declaring our own DataSource bean. Rather than replace the auto-configured DataSource bean, we can configure it via properties in application. yml or application.properties. Table 8.2 lists all of the properties that are useful for configuring the DataSource bean.

__Table 8.2 DataSource configuration properties__

| Property (prefixed with spring.datasource.) | Description                                                                                                                  |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| name                                        | The name of the data source                                                                                                  |
| initialize                                  | Whether or not to populate using data.sql (default: true)                                                                    |
| schema                                      | The name of a schema (DDL) script resource                                                                                   |
| data                                        | The name of a data (DML) script resource                                                                                     |
| sql-script-encoding                         | The character set for reading SQL scripts                                                                                    |
| platform                                    | The platform to use when reading the schema resource (for example, “schema-{platform}.sql”)                                  |
| continue-on-error                           | Whether or not to continue if initialization fails (default: false)                                                          |
| separator                                   | The separator in the SQL scripts (default: ;)                                                                                |
| driver-class-name                           | The fully qualified class name of the JDBC driver (can often be automatically inferred from the URL)                         |
| url                                         | The database URL                                                                                                             |
| username                                    | The database username                                                                                                        |
| password                                    | The database password                                                                                                        |
| jndi-name                                   | A JNDI name for looking up a datasource via JNDI                                                                             |
| max-active                                  | Maximum active connections (default: 100)                                                                                    |
| max-idle                                    | Maximum idle connections (default: 8)                                                                                        |
| min-idle                                    | Minimum idle connections (default: 8)                                                                                        |
| initial-size                                | The initial size of the connection pool (default: 10)                                                                        |
| validation-query                            | A query to execute to verify the connection                                                                                  |
| test-on-borrow                              | Whether or not to test a connection as it’s borrowed from the pool (default: false)                                          |
| test-on-return                              | Whether or not to test a connection as it’s returned to the pool (default: false)                                            |
| test-while-idle                             | Whether or not to test a connection while it is idle (default: false)                                                        |
| time-between-eviction-runs-millis           | How often (in milliseconds) to evict connections (default: 5000)                                                             |
| min-evictable-idle-time-millis              | The minimum time (in milliseconds) that a connection can be idle before being tested for eviction (default: 60000)           |
| max-wait                                    | The maximum time (in milliseconds) that the pool will wait when no connections are available before failing (default: 30000) |
| jmx-enabled                                 | Whether or not the data source is managed by JMX (default: false)                                                            |

Most of the properties in table 8.2 are for fine-tuning the connection pool. I’ll leave it to you to tinker with those settings as you see fit. What we’re interested in now, however, is setting a few properties that will point the DataSource bean at PostgreSQL instead of the embedded H2 database. Specifically, the spring.datasource.url, spring.datasource.username, and spring.datasource.password properties are what we need.

As I’m writing this, I have a PostgreSQL database running locally, listening on port 5432, with a username and password of “habuma” and “password”. Therefore, the following “production” profile in application.yml is what I used:

```
---
spring:
  profiles: production
  datasource:
    url: jdbc:postgresql://localhost:5432/readinglist
    username: habuma
    password: password
  jpa:
    database-platform: org.hibernate.dialect.PostgreSQLDialect
```

Notice that this excerpt starts with --- and the first property set is spring.profiles. This indicates that the properties that follow will only be applied if the “production” profile is active.

Next, the spring.datasource.url, spring.datasource.username, and spring.datasource.password properties are set. Note that it’s usually unnecessary to set the spring.datasource.driver-class-name property, as Spring Boot can infer it from the value of the spring.datasource.url property. I also had to set some JPA properties. The spring.jpa.database-platform property sets the underlying JPA engine to use Hibernate’s PostgreSQL dialect.

To enable this profile, we’ll need to set the spring.profiles.active property to “production”. There are several ways to set this property, but the most convenient way is by setting a system environment variable on the machine running the application server. To enable the “production” profile before starting Tomcat, I exported the SPRING_PROFILES_ACTIVE environment variable like this:

```
$ export SPRING_PROFILES_ACTIVE=production
```

You probably noticed that SPRING_PROFILES_ACTIVE is different from spring.profiles.active. It’s not possible to export an environment variable with periods in the name, so it was necessary to alter the name slightly. From Spring’s point of view, the two names are equivalent.

We’re almost ready to deploy the application to an application server and see it run. In fact, if you are feeling adventurous, go ahead and try it. You’ll run into a small problem, however.

By default, Spring Boot configures Hibernate to create the schema automatically when using the embedded H2 database. More specifically, it sets Hibernate’s hibernate.hbm2ddl.auto to create-drop, indicating that the schema should be created when Hibernate’s SessionFactory is created and dropped when it is closed.

But it’s set to do nothing if you’re not using an embedded H2 database. This means that our application’s tables won’t exist and you’ll see errors as it tries to query those nonexistent tables.

### 8.2.3 Enabling database migration

One option is to set the hibernate.hbm2ddl.auto property to create, create-drop, or update via Spring Boot’s spring.jpa.hibernate.ddl-auto property. For instance, to set hibernate.hbm2ddl.auto to create-drop we could add the following lines to application.yml:

```
spring:
  jpa:
    hibernate:
    ddl-auto: create-drop
```

This, however, is not ideal for production, as the database schema would be wiped clean and rebuilt from scratch any time the application was restarted. It may be tempting to set it to update, but even that isn’t recommended in production.

Alternatively, we could define the schema in schema.sql. This would work fine the first time, but every time we started the application thereafter, the initialization scripts would fail because the tables in question would already exist. This would force us to take special care in writing our initialization scripts to not attempt to repeat any work that has already been done.

A better choice is to use a database migration library. Database migration libraries work from a set of database scripts and keep careful track of the ones that have already been applied so that they won’t be applied more than once. By including the scripts within each deployment of the application, the database is able to evolve in concert with the application.

Spring Boot includes auto-configuration support for two popular database migration libraries:

* Flyway (http://flywaydb.org)
* Liquibase (www.liquibase.org)

All you need to do to use either of these database migration libraries with Spring Boot is to include them as dependencies in the build and write the scripts. Let’s see how they work, starting with Flyway.

#### DEFINING DATABASE MIGRATION WITH FLYWAY

Flyway is a very simple, open source database migration library that uses SQL for defining the migration scripts. The idea is that each script is given a version number, and Flyway will execute each of them in order to arrive at the desired state of the database. It also records the status of scripts it has executed so that it won’t run them again.

For the reading-list application, we’re starting with an empty database with no tables or data. Therefore, the script we’ll need to get started will need to create the Reader and Book tables, including any foreign-key constraints and initial data. Listing 8.2 shows the Flyway script we’ll need to go from an empty database to one that our application can use.

__Listing 8.2 A database initialization script for Flyway__

```
create table Reader (
  id serial primary key,
  username varchar(25) unique not null,
  password varchar(25) not null,
  fullname varchar(50) not null
);

create table Book (
  id serial primary key,
  author varchar(50) not null,
  description varchar(1000) not null,
  isbn varchar(10) not null,
  title varchar(250) not null,
  reader_username varchar(25) not null,
  foreign key (reader_username) references Reader(username)
);

create sequence hibernate_sequence;

insert into Reader (username, password, fullname)
            values ('craig', 'password', 'Craig Walls');
```

Create Reader table

Create Book table

Define a sequence

An initial Reader

As you can see, the Flyway script is just SQL. What makes it work with Flyway is where it’s placed in the classpath and how it’s named. Flyway scripts follow a naming convention that includes the version number, as illustrated in figure 8.1.

![图8.1](../Figures/figure-8.1.png)

__Figure 8.1 Flyway scripts are named with their version number.__



All Flyway scripts have names that start with a capital V which precedes the script’s version number. That’s followed by two underscores and a description of the script. Because this is the first script in the migration, it will be version 1. The description given can be flexible and is primarily to provide some understanding of the script’s purpose. Later, should we need to add a new table to the database or a new column to an existing table, we can create another script named with 2 in the version place.

Flyway scripts need to be placed in the path /db/migration relative to the application’s classpath root. Therefore, this script needs to be placed in src/main/resources/db/migration within the project.

You’ll also need to tell Hibernate to not attempt to create the tables by setting spring.jpa.hibernate.ddl-auto to none. The following lines in application.yml take care of that:

```
spring:
  jpa:
    hibernate:
      ddl-auto: none
```

All that’s left is to add Flyway as a dependency in the project build. Here’s the dependency that’s required for Gradle:

```
compile("org.flywaydb:flyway-core")
```

In a Maven build, the <dependency> is as follows:

```
<dependency>
  <groupId>org.flywayfb</groupId>
  <artifactId>flyway-core</artifactId>
</dependency>
```

When the application is deployed and running, Spring Boot will detect Flyway in the classpath and auto-configure the beans necessary to enable it. Flyway will step through any scripts in /db/migration and apply them if they haven’t already been applied. As each script is executed, an entry will be written to a table named schema_version. The next time the application starts, Flyway will see that those scripts have been recorded in schema_version and skip over them.

#### DEFINING DATABASE MIGRATION WITH LIQUIBASE

Flyway is simple to use, especially with help from Spring Boot auto-configuration. But defining migration scripts with SQL is a two-edged sword. Although it’s easy and natural to work with SQL, you run the risk of defining a migration script that works with one database platform but not another.

Rather than be limited to platform-specific SQL, Liquibase supports several formats for writing migration scripts that are agnostic to the underlying platform. These include XML, YAML, and JSON. And, if you really want it, Liquibase also supports SQL scripts.

The first step to using Liquibase with Spring Boot is to add it as a dependency in your build. The Gradle dependency is as follows:

```
compile("org.liquibase:liquibase-core")
```

For Maven, here’s the <dependency> you’ll need:

```
<dependency>
  <groupId>org.liquibase</groupId>
  <artifactId>liquibase-core</artifactId>
</dependency>
```

Spring Boot auto-configuration takes it from there, wiring up the beans that support Liquibase. By default, those beans are wired to look for all of the migration scripts in a single file named db.changelog-master.yaml in /db/changelog (relative to the classpath root). The migration script in listing 8.3 includes instructions to initialize the database for the reading-list application.

__Listing 8.3 A Liquibase script for initializing the reading-list database__

```
databaseChangeLog:
  - changeSet:
      id: 1
      author: habuma
      changes:
        - createTable:
            tableName: reader
            columns:
              - column:
                  name: username
                  type: varchar(25)
                  constraints:
                    unique: true
                    nullable: false
              - column:
                  name: password
                  type: varchar(25)
                  constraints:
                    nullable: false
              - column:
                  name: fullname
                  type: varchar(50)
                  constraints:
                    nullable: false
        - createTable:
            tableName: book
            columns:
              - column:
                  name: id
                  type: bigserial
                  autoIncrement: true
                  constraints:
                    primaryKey: true
                    nullable: false
              - column:
                  name: author
                  type: varchar(50)
                  constraints:
                    nullable: false
              - column:
                  name: description
                  type: varchar(1000)
                  constraints:
                    nullable: false
              - column:
                  name: isbn
                  type: varchar(10)
                  constraints:
                    nullable: false
              - column:
                  name: title
                  type: varchar(250)
                  constraints:
                    nullable: false
              - column:
                  name: reader_username
                  type: varchar(25)
                  constraints:
                    nullable: false
                    references: reader(username)
                    foreignKeyName: fk_reader_username
        - createSequence:
            sequenceName: hibernate_sequence
        - insert:
            tableName: reader
            columns:
              - column:
                  name: username
                  value: craig
              - column:
                  name: password
                  value: password
              - column:
                  name: fullname
                  value: Craig Walls
```

Changeset ID

Create reader table

Create book table

Define a sequence

Insert an initial reader

As you can see, the YAML format is a bit more verbose than the equivalent Flyway SQL script. But it’s still fairly clear as to its purpose and it isn’t coupled to any specific database platform.

Unlike Flyway, which has multiple scripts, one for each change set, Liquibase changesets are all collected in the same file. Note the id property on the line following the changeset command. Future changes to the database can be included by adding a new changeset with a different id. Also note that the id property isn’t necessarily numeric and may contain any text you’d like.

When the application starts up, Liquibase will read the changeset instructions in db.changelog-master.yaml, compare them with what it may have previously written to the databaseChangeLog table, and apply any changesets that have not yet been applied.

Although the example given here is expressed in YAML format, you’re welcome to choose one of Liquibase’s other supported formats, such as XML or JSON. Simply set the liquibase.change-log property (in application.properties or application.yml) to reflect the file you want Liquibase to load. For example, to use an XML changeset file, set liquibase.change-log like this:

```
liquibase:
  change-log: classpath:/db/changelog/db.changelog-master.xml
```

Spring Boot auto-configuration makes both Liquibase and Flyway a piece of cake to work with. But there’s a lot more to what each of these database migration libraries can do beyond what we’ve seen here. I encourage you to refer to each project’s documentation for more details.

We’ve seen how building Spring Boot applications for deployment into a conventional Java application server is largely a matter of creating a subclass of SpringBootServletInitializer and adjusting the build specification to produce a WAR file instead of a JAR file. But as we’ll see next, Spring Boot applications are even easier to build for the cloud.
