#第8章 部署Spring Boot应用程序

>本章内容

>-   部署WAR文件
- 数据库迁移
- 部署到云端

想一想你喜欢的动作电影。现在假设你要去电影院看这部电影，享受视听震撼。片中有高速追逐、爆炸和激战。好人还没战胜坏人，一切偏偏戛然而止。还没等影片里的冲突解决，电影院的灯亮了，大家都被领出门外。

虽然前面的铺垫很精彩，但电影的高潮才是最重要的。没有了它，就是为了动作而动作了。

现在，想象你正在开发应用程序，为解决某个业务问题投入了很多精力和创造力，但最终没能部署应用程序，没能让别人使用这个程序并乐在其中。当然，我们应用程序大多没有汽车追逐和爆炸（至少我希望是这样的），但一路上我们也会争分夺秒。当然，并非每行代码都为生产环境而写，但什么都不部署也挺让人失望的。

目前为止，我们的焦点都集中在使用Spring Boot的特性帮助大家开发应用程序。我们遇到了不少惊喜。但如果不越过终点线，应用程序没有部署，这一切都是徒劳。

在本章，我们会在使用Spring Boot开发应用程序的基础上更进一步，讨论如何部署那些应用程序。虽然这对部署过基于Java的应用程序的人来说并无特别之处，但Spring Boot和相关的Spring项目中有些独特的功能，基于这些功能我们可以让Spring Boot应用程序的部署变得与众不同。

实际上，大部分Java Web应用程序都以WAR文件的形式部署到应用服务器上。Spring Boot提供的部署方式则有所不同，后者在部署上提供了不少选择。在了解如何部署Spring Boot应用程序之前，让我们看看这些可选方式，找出能满足我们需求的那些选项。

## 8.1 衡量多种部署方式

Spring Boot应用程序有多种构建和运行方式，其中一些你已经使用过了。

- 在IDE中运行应用程序（涉及Spring ToolSuite或IntelliJ IDEA）。
- 使用Maven的`spring-boot:run`或Gradle的`bootRun`，在命令行里运行。
- 使用Maven或Gradle生成可运行的JAR文件，随后在命令行中运行。
- 使用Spring Boot CLI在命令行中运行Groovy脚本。
- 使用Spring Boot CLI来生成可运行的JAR文件，随后在命令行中运行。

这些选项每一个都适合运行正在开发的应用程序。但是，如果要将应用程序部署到生产环境或其他非开发环境中，又该怎么办呢？

虽然这些选项看起来没有一个能将应用部署于非开发环境，但事实上，它们之中只有一个选项不可用于生产环境——在IDE中运行应用显然不可取。可运行的JAR文件和Spring Boot CLI还是可以考虑的，两者还可以很好地将应用程序部署到云环境里。

也许你很想知道如何把Spring Boot应用程序部署到一个更加传统的应用服务器环境里，比如Tomcat、WebSphere或WebLogic。在这些情境中，可执行JAR文件和Groovy代码不适用。针对应用服务器的部署，你需要将应用程序打包成一个WAR文件。

实际上，Spring Boot应用程序可以用多种方式打包，详见表8-1。

__表8-1 Spring Boot部署选项__

| 部署产物 | 产生方式 | 目标环境 |
|---------|---------|----------|
| Groovy源码 | 手写 | Cloud Foundry及容器部署，比如Docker |
| 可执行JAR | Maven、Gradle或Spring Boot CLI | 云环境，包括Cloud Foundry和Heroku，还有容器部署，比如Docker |
| WAR | Maven或Gradle | Java应用服务器或云环境，比如Cloud Foundry |

如你所见，在做最终选择时需要考虑目标环境。如果要将应用程序部署到自己数据中心的Tomcat服务器上，WAR文件就是你的选择。另一方面，如果要部署到Cloud Foundry，你可以使用表里列出的各种选项。

在本章，我们将关注以下这些选项。

- 向Java应用服务器里部署WAR文件。
- 向Cloud Foundry里部署可执行JAR文件。
- 向Heroku里部署可执行JAR文件（构建过程是由Heroku执行的）。

探索这些场景的时候，我们还要处理一件事。在开发应用程序时我们使用了嵌入式的H2数据库，现在得把它替换成生产环境所需的数据库了。

首先，让我们看看如何将阅读列表应用程序构建为WAR文件。这样才能把它部署到Java应用服务器里，比如Tomcat、WebSphere或WebLogic。

## 8.2 部署到应用服务器

到目前为止，阅读列表应用程序每次运行，Web应用程序都通过内嵌在应用里的Tomcat提供服务。情况和传统Java Web应用程序正好相反。不是应用程序部署在Tomcat里，而是Tomcat部署在了应用程序里。

归功于Spring Boot的自动配置功能，我们不需要创建web.xml文件或者Servlet初始化类来声明Spring MVC的`DispatcherServlet`。但如果要将应用程序部署到Java应用服务器里，我们就需要构建WAR文件了。这样应用服务器才能知道如何运行应用程序。那个WAR文件里还需要一个对Servlet进行初始化的东西。

### 8.2.1 构建WAR文件

实际上，构建WAR文件并不困难。如果你使用Gradle来构建应用程序，只需应用WAR插件即可：
```
apply plugin: 'war'
```  
随后，在build.gradle里用以下`war`配置替换原来的`jar`配置：
```
war {
  baseName = 'readinglist'
  version = '0.0.1-SNAPSHOT'
}
```
两者的唯一区别就是_j_换成了_w_。

如果你使用Maven来构建项目，获取WAR文件就更容易了。只需把`<packaging>`元素的值从`jar`改为`war`。
```
<packaging>war</packaging>
```
这样就能生成WAR文件了。但如果WAR文件里没有启用Spring MVC `DispatcherServlet`的web.xml文件或者Servlet初始化类，这个WAR文件就一无是处。

此时就该Spring Boot出马了。它提供的`SpringBootServletInitializer`是一个支持Spring Boot的Spring `WebApplicationInitializer`实现。除了配置Spring的`DispatcherServlet`，`SpringBootServletInitializer`还会在Spring应用程序上下文里查找`Filter`、`Servlet`或`ServletContextInitializer`类型的Bean，把它们绑定到Servlet容器里。

要使用`SpringBootServletInitializer`，只需创建一个子类，覆盖`configure()`方法来指定Spring配置类。代码清单8-1是`ReadingListServletInitializer`，也就是我们为阅读列表应用程序写的`SpringBootServletInitializer`的子类。

__代码清单8-1 为阅读列表应用程序扩展`SpringBootServletInitializer`__

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

>文字

>Specify Spring configuration：指定Spring配置

如你所见，`configure()`方法传入了一个`SpringApplicationBuilder`参数，并将其作为结果返回。期间它调用`sources()`方法注册了一个Spring配置类。本例只注册了一个`Application`类。回想一下，这个类既是启动类（带有`main()`方法），也是一个Spring配置类。

虽然阅读列表应用程序里还有其他Spring配置类，但没有必要在这里把它们全部注册进来。`Application`类上添加了`@SpringBootApplication`注解。这会隐性开启组件扫描，而组件扫描则会发现并应用其他配置类。

现在我们可以构建应用程序了。如果使用Gradle，你只需调用`build`任务即可：
```
$ gradle build
```
没问题的话，你可以在build/libs里看到一个名为readinglist-0.0.1-SNAPSHOT.war的文件。

对于基于Maven的项目，可以使用`package`：
```
$ mvn package
```  
成功构建之后，你可以在target目录里找到WAR文件。

剩下的工作就是部署应用程序了。应用服务器不同，部署过程会有所区别，因此请参考应用服务器的部署说明文档。

对于Tomcat而言，可以把WAR文件复制到Tomcat的webapps目录里。如果Tomcat正在运行（要是没有运行，则在下次启动时检测），则会检测到WAR文件，解压并进行安装。

假设你没有在部署前重命名WAR文件，而Servlet上下文路径与WAR文件的主文件名相同，在本例中是/readinglist-0.0.1-SNAPSHOT。用你的浏览器打开[http://server:port/readinglist-0.0.1-SNAPSHOT](http://server:port/readinglist-0.0.1-SNAPSHOT)就能访问应用程序了。

还有一点值得注意：就算我们在构建的是WAR文件，这个文件仍旧可以脱离应用服务器直接运行。如果你没有删除`Application`里的`main()`方法，构建过程生成的WAR文件仍可直接运行，一如可执行的JAR文件：
```
$ java -jar readinglist-0.0.1-SNAPSHOT.war
```
这样一来，同一个部署产物就能有两种部署方式了！

现在，应用程序应该已经在Tomcat里顺利地运行起来了。但是它还在使用内嵌的H2数据库。开发应用程序时，嵌入式数据库很好用，但对生产环境而言这不是一个明智的选择。让我们来看看如何在部署到生产环境时选择不同的数据源。

### 8.2.2 创建生产Profile

多亏了自动配置，我们有了一个指向嵌入式H2数据库的`DataSource` Bean。更确切地说，`DataSource` Bean是一个数据库连接池，通常是`org.apache.tomcat.jdbc.pool.DataSource`。因此，很明显，要使用嵌入式H2之外的数据库，我们只需声明自己的`DataSource` Bean，指向我们选择的生产数据库，用它覆盖自动配置的`DataSource` Bean。

例如，假设我们想使用运行localhost上的PostgreSQL数据库，数据库名字是readingList。下面的`@Bean`方法就能声明我们的`DataSource` Bean：
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
这里`DataSource`的类型是Tomcat的`org.apache.tomcat.jdbc.pool.DataSource`，不要和`javax.sql.DataSource`搞混了。前者是后者的实现。连接数据库所需的细节（包括JDBC驱动类名、数据库URL、用户名和密码）提供给了`DataSourse`实例。声明了这个Bean之后，默认自动配置的`DataSource` Bean就会忽略。

这个`@Bean`方法最关键的一点是，它还添加了`@Profile`注解，说明只有在`production`Profile被激活时才会创建该Bean。所以，在开发时我们还能继续使用嵌入式的H2数据库。激活`production`Profile后就能使用PostgreSQL数据库了。

虽然这么做能达到目的，但是配置数据库细节的时候，最好还是不要显式地声明自己的`DataSource` Bean。在不替换自动配置的`Datasource` Bean的情况下，我们还能通过application.yml或application.properties来配置数据库的细节。表8-2列出了在配置`DataSource` Bean时用到的全部属性。

__表8-2 `DataSource`配置属性__

| 属性（带有spring.datasource.前缀） | 描述 |
|-----------------------------------|-----|
| `name` | 数据源的名称 |
| `initialize` | 是否执行data.sql（默认：`true`） |
| `schema` | Schema（DDL）脚本资源的名称 |
| `data` | 数据（DML）脚本资源的名称 |
| `sql-script-encoding` | 读入SQL脚本的字符集 |
| `platform` | 读入Schema资源时所使用的平台（例如：schema-{platform}.sql） |
| `continue-on-error` | 如果初始化失败是否还要继续（默认：`false`） |
| `separator` | SQL脚本的分隔符（默认：`;`） |
| `driver-class-name` | JDBC驱动的全限定类名（通常能通过URL自动推断出来） |
| `url` | 数据库URL |
| `username` | 数据库的用户名 |
| `password` | 数据库的密码 |
| `jndi-name` | 通过JNDI查找数据源的JNDI名称 |
| `max-active` | 最大的活跃连接数（默认：`100`） |
| `max-idle` | 最大的闲置连接数（默认：`8`） |
| `min-idle` | 最小的闲置连接数（默认：`8`） |
| `initial-size` | 连接池的初始大小（默认：`10`） |
| `validation-query` | 用来验证连接的查询语句 |
| `test-on-borrow` | 从连接池借用连接时是否检查连接（默认：`false`） |
| `test-on-return` | 向连接池归还连接时是否检查连接（默认：`false`） |
| `test-while-idle` | 连接空闲时是否测试连接（默认：`false`） |
| `time-between-eviction-runs-millis` | 多久（单位为毫秒）清理一次连接（默认：`5000`） |
| `min-evictable-idle-time-millis` | 在被测试是否要清理前，连接最少可以空闲多久（单位为毫秒，默认：`60000`） |
| `max-wait` | 当没有可用连接时，连接池在返回失败前最多等多久（单位为毫秒，默认：`30000`） |
| `jmx-enabled` | 数据源是否可以通过JMX进行管理（默认：`false`） |

表8-2里的大部分属性都是用来微调连接池的。怎么设置这些属性以适应你的需要，这就交给你来解决了。我们现在要设置属性，让`DataSource` Bean指向PostgreSQL而非内嵌的H2数据库。具体来说，我们要设置的是`spring.datasource.url`、`spring.datasource.username`以及`spring.datasource.password`属性。

在设置这些内容时，我在本地运行了一个PostgreSQL数据库，监听5432端口。用户名和密码分别是habuma和password。因此，application.yml的`production` Profile里需要如下内容：

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

请注意，这个代码片段以`---`开头，设置的第一个属性是`spring.profiles`。这说明随后的属性都只在`production`Profile激活时才会生效。

随后设置的是`spring.datasource.url`、`spring.datasource.username`和`spring.datasource.password`属性。注意，`spring.datasource.driver-class-name`属性一般无需设置。Spring Boot可以根据`spring.datasource.url`属性的值做出相应推断。我还设置了一些JPA的属性。`spring.jpa.database-platform`属性将底层的JPA引擎设置为Hibernate的PostgreSQL方言。

要开启这个Profile，我们需要把`spring.profiles.active`属性设置为`production`。实现方式有很多，但最方便的还是在运行应用服务器的机器上设置一个系统环境变量。在启动Tomcat前开启`production`Profile，我需要像这样设置`SPRING_PROFILES_ACTIVE`环境变量：
```
$ export SPRING_PROFILES_ACTIVE=production
```
你也许已经注意到了，`SPRING_PROFILES_ACTIVE`不同于`spring.profiles.active`。因为无法在环境变量名里使用句点，所以变量名需要稍作修改。站在Spring的角度看，这两个名字是等价的。

我们基本已经可以在应用服务器上部署并运行应用程序了。实际上，如果你喜欢冒险，也可以直接尝试一下。不过你会遇到一点小问题。

默认情况下，在使用内嵌的H2数据库时，Spring Boot会配置Hibernate来自动创建Schema。更确切地说，这是将Hibernate的`hibernate.hbm2ddl.auto`设置为`create-drop`，说明在Hibernate的`SessionFactory`创建时会创建Schema，`SessionFactory`关闭时删除Schema。

但如果没使用内嵌的H2数据库，那么它什么都不会做。也就是，说应用程序的数据表尚不存在，在查询那些不存在的表时会报错。

### 8.2.3 开启数据库迁移

一种途径是通过Spring Boot的`spring.jpa.hibernate.ddl-auto`属性将`hibernate.hbm2ddl.auto`属性设置为`create`、`create-drop`或`update`。例如，要把`hibernate.hbm2ddl.auto`设置为`create-drop`，我们可以在application.yml里加入如下内容：
```
spring:
  jpa:
    hibernate:
      ddl-auto: create-drop
```
然而，这对生产环境来说并不理想，因为应用程序每次重启数据库，Schema就会被清空，从头开始重建。它可以设置为`update`，但就算这样，我们也不建议将其用于生产环境。

还有一个途径。我们可以在schema.sql里定义Schema。在第一次运行时，这么做没有问题，但随后每次启动应用程序时，这个初始化脚本都会失败，因为数据表已经存在了。这就要求在书写初始化脚本时格外注意，不要重复执行那些已经做过的工作。

一个比较好的选择是使用数据库迁移库（database migration library）。它使用一系列数据库脚本，而且会记录哪些已经用过了，不会多次运用同一个脚本。应用程序的每个部署包里都包含了这些脚本，数据库可以和应用程序保持一致。

Spring Boot为两款流行的数据库迁移库提供了自动配置支持。

* Flyway（[http://flywaydb.org](http://flywaydb.org)）
* Liquibase（[http://www.liquibase.org](http://www.liquibase.org)）

当你想要在Spring Boot里使用其中某一个库时，只需在项目里加入对应的依赖，然后编写脚本就可以了。让我们先从Flyway开始了解吧。

####1.用Flyway定义数据库迁移过程

Flyway是一个非常简单的开源数据库迁移库，使用SQL来定义迁移脚本。它的理念是，每个脚本都有一个版本号，Flyway会顺序执行这些脚本，让数据库达到期望的状态。它也会记录已执行的脚本状态，不会重复执行。

在阅读列表应用程序这里，我们先从一个没有数据表和数据的空数据库开始。因此，这个脚本里需要先创建`Reader`和`Book`表，包含外键约束和初始化数据。代码清单8-2就是从空数据库到可用状态的Flyway脚本。

__代码清单8-2 Flyway数据库初始脚本__

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

>文字：

>Create Reader table：创建`Reader`表

>Create Book table：创建`Book`表

>Define a sequence：定义序列

>An initial Reader：`Reader`的初始数据

如你所见，Flyway脚本就是SQL。让其发挥作用的是其在Classpath里的位置和文件名。Flyway脚本都遵循一个命名规范，含有版本号，具体如图8-1所示。

>P169图

__图8-1 用版本号命名的Flyway脚本__

所有Flyway脚本的名字都以大写字母V开头，随后是脚本的版本号。后面跟着两个下划线和对脚本的描述。因为这是整个迁移过程中的第一个脚本，所以它的版本是1。描述可以很灵活，主要用来帮助理解脚本的用途。稍后我们需要向数据库添加新表，或者向已有数据表添加新字段。可以再创建一个脚本，标明版本号为2。

Flyway脚本需要放在相对于应用程序Classpath根路径的/db/migration路径下。因此，项目中，脚本需要放在src/main/resources/db/migration里。

你还需要将`spring.jpa.hibernate.ddl-auto`设置为`none`，由此告知Hibernate不要创建数据表。这关系到application.yml中的如下内容：

```
spring:
  jpa:
    hibernate:
      ddl-auto: none
```
剩下的就是将Flyway添加为项目依赖。在Gradle里，此依赖是这样的：
```
compile("org.flywaydb:flyway-core")
```
在Maven项目里，`<dependency>`是这样的：
```
<dependency>
  <groupId>org.flywayfb</groupId>
  <artifactId>flyway-core</artifactId>
</dependency>
```
在应用程序部署并运行起来后，Spring Boot会检测到Classpath里的Flyway，自动配置所需的Bean。Flyway会依次查看/db/migration里的脚本，如果没有执行过就运行这些脚本。每个脚本都执行过后，向schema_version表里写一条记录。应用程序下次启动时，Flyway会先看schema_version里的记录，跳过那些脚本。

####2.用Liquibase定义数据库迁移过程

Flyway用起来很简便，在Spring Boot自动配置的帮助下尤其如此。但是，使用SQL来定义迁移脚本是一把双刃剑。SQL用起来便捷顺手，却要冒着只能在一个数据库平台上使用的风险。

Liquibase并不局限于针对平台的SQL，支持书写迁移脚本的多种格式，不用关心底层平台（其中包括XML、YAML和JSON）。如果你有这个期望的话，Liquibase当然也支持SQL脚本。

要在Spring Boot里使用Liquibase，第一步是添加依赖。Gradle里的依赖是这样的：

```
compile("org.liquibase:liquibase-core")
```

对于Maven项目，你需要添加如下`<dependency>`：

```
<dependency>
  <groupId>org.liquibase</groupId>
  <artifactId>liquibase-core</artifactId>
</dependency>
```

有了这个依赖，Spring Boot自动配置就能接手，配置好用于支持Liquibase的Bean。默认情况下，那些Bean会在/db/changelog（相对于Classpath根目录）里查找db.changelog-master.yaml文件。这个文件里都是迁移脚本。代码清单8-3的初始化脚本为阅读列表应用程序进行了数据库初始化。

__代码清单8-3 用于阅读列表数据库的Liquibase初始化脚本__

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

>文字

> Changeset ID：变更集ID

> Create reader table：创建`reader`表

> Create book table：创建`book`表

> Define a sequence：定义序列

> Insert an initial reader：插入`reader`的初始记录

如你所见，比起等效的Flyway SQL脚本，YAML格式略显繁琐，但看起来还是很清晰的，而且这个脚本不与任何特定的数据库平台绑定。

与Flyway不同，Flyway有多个脚本，每个脚本对应一个变更集。Liquibase变更集都集中在一个文件里。请注意，`changeset`命令后的那行有一个`id`属性，要对数据库进行后续变更。可以添加一个新的`changeset`，只要`id`不一样就行。此外，`id`属性也不一定是数字，可以包含任意内容。

应用程序启动时，Liquibase会读取db.changelog-master.yaml里的变更集指令集，与之前写入`databaseChangeLog`表里的内容做对比，随后执行未运行过的变更集。

虽然这里的例子使用的是YAML格式，但你也可以任意选择Liquibase所支持的其他格式，比如XML或JSON。只需简单地设置`liquibase.change-log`属性（在application.properties或application.yml里），标明希望Liquibase加载的文件即可。举个例子，要使用XML变更集，可以这样设置`liquibase.change-log`：

```
liquibase:
  change-log: classpath:/db/changelog/db.changelog-master.xml
```

Spring Boot的自动配置让Liquibase和Flyway的使用变得轻而易举。但实际上所有数据库迁移库都有更多功能，这里不便一一列举。建议大家参考官方文档，了解更多详细内容。

我们已经了解了如何将Spring Boot应用程序部署到传统的Java应用服务器上，基本就是创建一个`SpringBootServletInitializer`的子类，调整构建说明来生成一个WAR文件，而非JAR文件。接下来我们会看到，Spring Boot应用程序在云端使用更方便。

## 8.3 推上云端

服务器硬件的购买和维护成本很高。大流量很难通过适当扩展服务器去处理，这种做法在某些组织中甚至是禁忌。现如今，相比在自己的数据中心运行应用程序，把它们部署到云上是更引人注目，而且划算的做法。

目前有多个云平台可供选择，而那些提供Platform as a Service（PaaS）能力的平台无疑是最有吸引力的。PaaS提供了现成的应用程序部署平台，带有附加服务（比如数据库和消息代理），可以绑定到应用程序上。除此之外，当你的应用程序要求提供更大的马力时，云平台能轻松实现应用程序在运行时向上（或向下）伸缩，只需添加或删除实例即可。

之前我们已经把阅读列表应用程序部署到了传统的应用服务器上，现在再试试将其部署到云上。我们将把应用程序部署到Cloud Foundry和Heroku这两个著名的PaaS平台上。

### 8.3.1 部署到Cloud Foundry

Cloud Foundry是Pivotal的PaaS平台。这家公司也赞助了Spring Framework和Spring平台里的其他库。Cloud Foundry里最吸引人的特点之一就是它既有开源版本，也有多个商业版本。你可以选择在何处运行Cloud Foundry。它甚至还可以在公司数据中心的防火墙后运行，提供私有云。

我打算将阅读列表应用程序部署到Pivotal Web Services（PWS）上。这是一个由Pivotal托管的公共Cloud Foundry，地址是[http://run.pivotal.io](http://run.pivotal.io)。如果想使用PWS，你可以注册一个账号。PWS提供为期60天的免费试用，在试用期间无需提交任何信用卡信息。

在注册了PWS后，可以从[https://console.run.pivotal.io/tools](https://console.run.pivotal.io/tools)下载并安装`cf`命令行工具。你可以通过这个工具将应用程序推上Cloud Foundry。但你要先用这个工具登录自己的PWS账号。

```
$ cf login -a https://api.run.pivotal.io
API endpoint: https://api.run.pivotal.io

Email> {your email}

Password> {your password}
Authenticating...
OK
```
现在我们已经可以把阅读列表应用程序传到云上了。实际上，我们的项目已经做好了部署到Cloud Foundry的准备，只需使用`cf push`命令把它推上去就好。
```
$ cf push sbia-readinglist -p build/libs/readinglist.war
```
`cf push`命令的第一个参数指定了应用程序在Cloud Foundry里的名称。这个名称将被用作托管应用程序的子域名。本例中，应用程序的完整域名将是[http://sbia-readinglist.cfapps.io](http://sbia-readinglist.cfapps.io)。因此，应用程序的命名很重要。名字必须独一无二，这样才不会和Cloud Foundry里部署的其他应用程序（包括其他用户部署的应用程序）发生冲突。

因为空想一个独一无二的名称有点困难，所以`cf push`命令提供了一个`--random-route`选项，可以为你随机产生一个子域名。下面的例子演示了如何上传阅读列表应用程序，生成一个随机的子域名。

```
$ cf push sbia-readinglist -p build/libs/readinglist.war --random-route
```

在使用了`--random-route`后，还是要设定应用程序名称。会有两个随机选择的单词添加到后面，组成子域名。（在我自己尝试的时候，生成的子域名是sbia-readinglist-gastroenterological-stethoscope。）

> __不仅是WAR文件__

>虽然我们部署的应用程序是一个WAR文件，但Cloud Foundry也可以部署其他格式的Spring Boot应用程序，包括可执行的JAR文件，甚至Spring Boot CLI开发的未经编译的Groovy脚本。

如果一切顺利，我们部署的应用程序应该可以处理请求。假设子域名是sbia-readinglist，你可以用浏览器访问[http://sbia-readinglist.cfapps.io](http://sbia-readinglist.cfapps.io)，看看效果。你应该会被引导到登录页。回想一下，数据库迁移脚本中插入了一个名为craig的用户，密码是password，可以以此登录应用程序。

你可以在应用程序里随便点点，加几本书。所有的东西都可以运行，但还是有点不对劲。如果重启应用程序（通过`cf restart`命令），重新登录，你会发现阅读列表清空了。你在重启前添加的书都不见了。

应用程序重启后数据消失，原因在于内嵌的H2数据库还在使用。我们可以通过Actuator的/health端点验证推测。它返回的信息大约是这样的：
```
{
  "status": "UP",
  "diskSpace": {
    "status": "UP",
    "free": 834236510208,
    "threshold": 10485760
  }, "db": {
    "status": "UP",
    "database": "H2",
    "hello": 1
  }
}
```
请注意`db.database`属性的值。它证实了我们之前的怀疑——果然用的是内嵌的H2数据库。我们需要修复这个问题。

实际上，Cloud Foundry以市集服务（marketplace services）的形式提供了一些数据库以供选择，包括MySQL和PostgreSQL。因为我们已经在项目里放了PostgreSQL的JDBC驱动，所以就使用市集里的PostgreSQL服务，名字是elephantsql。

elephantsql服务也有不少计划可选，小到开发用的小型数据库，大到工业级生产数据库。elephantsql的完整计划列表可以通过`cf marketplace`命令获得。
```
$ cf marketplace -s elephantsql
Getting service plan information for service elephantsql as craig@habuma.com... OK

service plan		   description			       free or paid
turtle		             Tiny Turtle	                 free
panda	               Pretty Panda	                   paid
hippo	                Happy Hippo	                   paid
elephant              Enormous Elephant                    paid
```
如你所见，比较严谨的生产级数据库计划都是要付费的。你可以选择你所期望的计划。我先假设你会选择免费的turtle。

创建数据库服务的实例，需要使用`cf create-service`命令，指定服务名、计划名和实例名。
```
$ cf create-service elephantsql turtle readinglistdb
Creating service readinglistdb in org habuma /
      space development as craig@habuma.com...
OK
```
服务创建后，需要通过`cf bind-service`命令将它绑定到我们的应用程序上。
```
$ cf bind-service sbia-readinglist readinglistdb
```  
将一个服务绑定到应用程序上不过就是为应用程序提供了连接服务的细节，这里用的是名为`VCAP_SERVICES`的环境变量。它不会通过修改应用程序来使用服务。

我们可以改写阅读列表应用程序，读取`VCAP_SERVICES`，使用其中提供的信息来连接数据库服务。但其实完全不用这么做。实际上，我们只需用`cf restage`命令重启应用程序就可以了：
```
$ cf restage sbia-readinglist
```
`cf restage`命令会让Cloud Foundry重新部署应用程序，并重新计算`VCAP_SERVICES`的值。如此一来，我们的应用程序会在Spring应用程序上下文里声明一个引用了绑定数据库服务的`DataSource` Bean，用它来替换原来的`DataSource` Bean。这样我们就能抛开内嵌的H2数据库，使用elephantsql提供的PostgreSQL服务了。

现在来试一下。登录应用程序，添加几本书，然后重启。重启之后你所添加的书应该还在列表里，因为它们已经被持久化在绑定的数据库服务里，而非内嵌的H2数据库里。再访问一下Actuator的/health端点，返回的内容能证明我们在使用PostgreSQL：
```
{
  "status": "UP",
  "diskSpace": {
    "status": "UP",
    "free": 834331525120,
    "threshold": 10485760
  }, "db": {
    "status": "UP",
    "database": "PostgreSQL",
    "hello": 1
  }
}
```
Cloud Foundry对Spring Boot应用程序部署而言是极佳的PaaS，Cloud Foundry与Spring项目搭配可谓如虎添翼。但Cloud Foundry并非Spring Boot应用程序在PaaS方面的唯一选择。让我们来看看如何将阅读列表应用程序部署到另一个流行的Paas平台：Heroku。

### 8.3.2 部署到Heroku

Heroku在应用程序部署上有一套独特的方法，不用部署完整的部署产物。Heroku为你的应用程序安排了一个Git仓库。每当你向仓库里提交代码时，它都会自动为你构建并部署应用程序。

如果还是解决不了问题，则需要先将项目目录初始化为Git仓库。
```
$ git init
```
这样Heroku的命令行工具就能自动把远程Heroku Git仓库添加到项目里。

现在可以通过Heroku的命令行工具在Heroku中设置应用程序了。这里使用`apps:create`命令。
```
$ heroku apps:create sbia-readinglist
```
这里我要求Heroku将应用程序命名为sbia-readinglist。这将成为Git仓库的名字，同时也是应用程序在herokuapps.com的子域名。你需要确定这个名字唯一，因为不能有同名应用程序。此外，你也可以让Heroku来替你生成一个独特的名字（比如fierce-river-8120或serene-anchorage-6223）。

`apps:create`命令会在[https://git.heroku.com/sbia-readinglist.git](https://git.heroku.com/sbia-readinglist.git)创建一个远程Git仓库，并在本地项目的Git配置里添加一个名为heroku的远程仓库引用。有了它就能通过`git`命令将我们的项目推送到Heroku了。

Heroku里的项目已经设置完毕，但我们现在还不能进行推送。Heroku需要你提供一个名为Procfile的文件，告诉Heroku应用程序构建后该如何运行。对于阅读列表应用程序而言，我们需要告诉Heroku，构建生成的WAR文件要当作可执行JAR文件来运行，这里使用`java`命令。***{![当前使用的项目会实际生成一个可执行的WAR文件。但对Heroku来说，它和可执行的JAR文件没什么区别。]}***假设应用程序是用Gradle来构建的，只需要如下一行内容的Procfile：
```
web: java -Dserver.port=$PORT -jar build/libs/readinglist.war
```
另一方面，如果你使用Maven来构建项目，JAR文件的路径就会有所不同。Heroku需要到target目录，而不是build/libs目录里寻找可执行WAR文件。具体如下：
```
web: java -Dserver.port=$PORT -jar target/readinglist.war
```
不管何种情况，你都需要像例子中那样设置`server.port`属性。这样内嵌的Tomcat服务器才能在Heroku分配的端口上（通过`$PORT`变量指定）启动。

我们差不多可以把应用程序推上Heroku了，但Gradle构建说明还要稍作调整。Heroku构建应用程序时，会执行一个名为`stage`的任务，因此需要在build.gradle里添加这个`stage`任务。
```
task stage(dependsOn: ['build']) {
}
```
如你所见，这个`stage`任务什么也没做，但依赖了`build`任务。于是，在Heroku使用`stage`任务构建应用程序会触发`build`任务，生成的JAR文件会放在build/libs目录里。

你还需要告诉Heroku用什么Java版本来构建并运行应用程序。这样Heroku才能用合适的版本来运行它。最简单的方法是在项目根目录里创一个名为system.properties的文件，在其中设置`java.runtime.version`属性：
```
java.runtime.version=1.7
```
现在就可以将项目推上Heroku了。和前面说一样，只需将代码推到远程Git仓库，Heroku会帮我们搞定其他事情。
```
$ git commit -am "Initial commit"
$ git push heroku master
```  
然后，Heroku会根据找到的构建说明文件，使用Maven或Gradle进行构建，再用`Procfile`里的指令来运行应用程序。就绪后，你可以用浏览器打开[http://{app name}.herokuapp.com](http://{app name}.herokuapp.com)。这里的{app name}就是你在`apps:create`里给应用程序起的名字。例如，我在部署时将应用程序命名为sbia-readinglist，所以它的URL就是[http://sbia-readinglist.herokuapps.com](http://sbia-readinglist.herokuapps.com)。

你可以在应用程序里随便点点，但要访问一下/health端点。`db.database`属性会告诉你应用程序正在使用内嵌的H2数据库。我们应该把它换成PostgreSQL服务。

我们可以通过Heroku命令行工具的`addons:add`命令创建并绑定一个PostgreSQL服务。
```
$ heroku addons:add heroku-postgresql:hobby-dev
```
这里我们要使用名为`heroku-postgresql`的附加服务。这是Heroku提供的PostgreSQL服务。我们还要求使用该服务的`hobby-dev`计划，这是免费的。

在PostgreSQL服务创建并绑定到应用程序后，Heroku会自动重启应用程序以保证绑定生效。但即便如此，我们在访问/health端点时仍然会看到应用程序还在使用内嵌的H2数据库。那是因为H2的自动配置仍然有效，谁也没告诉Spring Boot要用PostgreSQL代替H2。

一个办法是设置`spring.datasource.*`属性，做法和我们将应用程序部署到应用服务器上时一样。我们所需要的信息能在数据库服务的仪表板上找到，可以用`addons:open`命令打开仪表板。
```
$ heroku addons:open waking-carefully-3728
```  
在这个例子里，数据库实例的名字是waking-carefully-3728。该命令会在Web浏览器里打开仪表板页面，其中包含了你所需要的全部连接信息，包括主机名、数据库名和账户信息。总之，设置`spring.datasource.*`属性所需的一切信息都在这里了。

还有一个更简单的办法，与其自己查找那些信息，再设置到属性里，为什么不让Spring替我们查找信息呢？实际上，这就是Spring Cloud Connectors的用途。它可以用在Cloud Foundry和Heroku上，查找绑定到应用程序上的所有服务，并自动配置应用程序，以便使用那些服务。

我们只需在项目中加入Spring Cloud Connectors依赖即可。在Gradle项目里，在build.gradle中添加如下内容：
```
compile( "org.springframework.boot:spring-boot-starter-cloud-connectors")
```
如果你用的是Maven，则添加如下Spring Cloud Connectors`<dependency>`：
```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-cloud-connectors</artifactId>
</dependency>
```
只有激活`cloud` Profile，Spring Cloud Connectors才会工作。要在Heroku里激活`cloud` Profile，可以使用`config:set`命令：
```
$ heroku config:set SPRING_PROFILES_ACTIVE="cloud"
```
现在项目里有了Spring Cloud Connectors依赖，`cloud` Profile也激活了。我们可以再推一次应用程序。
```
$ git commit -am "Add cloud connector"
$ git push heroku master
```
应用程序启动后，登入应用程序，查看/health端点。它应该显示应用程序已经连接到了PostgreSQL数据库：
```
"db": {
  "status": "UP",
  "database": "PostgreSQL",
  "hello": 1
}
```
现在我们的应用程序已经部署到云上，可以接受世界各地的请求了！

## 8.4 小结

Spring Boot应用程序的部署有多种方式，包括使用传统的应用服务器和云上的PaaS平台。在本章，我们了解了其中的一些部署方式，把阅读列表应用程序以WAR文件的方式部署到Tomcat和云上（Cloud Foundry和Heroku）。

Spring Boot应用程序的构建说明经常会配置为生成可执行的JAR文件。我们也看到了如何对构建进行微调，如何编写一个`SpringBootServletInitializer`实现，生成WAR文件，以便部署到应用服务器上。

随后，我们进一步了解了如何将应用程序部署到Cloud Foundry上。Cloud Foundry非常灵活，能够接受各种形式的Spring Boot应用程序，包括可执行JAR文件、传统WAR文件，甚至还包括原始的Spring Boot CLI Groovy脚本。我们还了解了Cloud Foundry如何自动将内嵌式数据源替换为绑定到应用程序上的数据库服务。

虽然Heroku不能像Cloud Foundry那样自动替换数据源的Bean，但在本章最后，我们还是看到了如何通过添加Spring Cloud Foundry库来实现一样的效果。这里使用绑定的数据库服务，而非内嵌式数据库。

在本章，我们还了解了如何在Spring Boot里使用Flyway和Liquibase这样的数据库迁移工具。在初次部署应用程序时，我们通过数据库迁移的方式完成了数据库的初始化，在后续的部署过程中，我们可以按需修改数据库。
