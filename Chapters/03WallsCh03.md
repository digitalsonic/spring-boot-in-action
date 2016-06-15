# 第3章 自定义配置

> 本章内容

>- 覆盖自动配置的Bean
- 用外置属性进行配置
- 自定义错误页

能自由选择真是太棒了，如果你订过披萨（有没订过的么？）就会知道，你完全可以掌控薄饼上放哪些辅料。选定腊肠、意大利辣香肠、青辣椒和额外芝士的时候，你就是在配置披萨，把它变成满足你个人喜好的食物。

另一方面，大部分披萨店也提供某种形式的自动配置，你可以点肉食者的披萨、素食者的披萨、香辣意大利披萨，或者是自动配置披萨中的极品——至尊披萨（supreme pizza）。在下单时，你并没有指定具体的辅料，你所点的披萨种类决定了所用的辅料。

但如果你想要至尊披萨上的全部辅料，还想要加墨西哥胡椒，但又不想放蘑菇该怎么办？你偏爱辣食又不喜欢吃菌类，自动配置不适合你的口味，你就只能自己烹制披萨了吗？当然不是，大部分披萨店会让你以菜单上已有的选项为基础进行定制。

使用传统Spring配置的过程，就如同订披萨的时候自己指定全部的辅料。你可以完全掌控Spring配置的内容，可是显式声明应用程序里全部的Bean并不是明智之举。而Spring Boot自动配置就像是从菜单上选份特定的披萨，让Spring Boot处理各种细节比自己声明上下文里全部的Bean要容易很多。

幸运的是，Spring Boot自动配置非常灵活，就像披萨厨师可以不在你的披萨里放蘑菇，而是加墨西哥胡椒一样，Spring Boot能让你参与进来，影响自动配置的实施。

本章我们将看到两种影响自动配置的方式——使用显式配置进行覆盖和使用属性进行精细化配置，还会看到如何使用Spring Boot提供的钩子引入自定义的错误页。

## 3.1 覆盖Spring Boot自动配置

一般来说，如果不用配置就能得到和显式配置一样的结果，那么不写配置是最直接的选择。既然如此，那干嘛还要多做额外的工作呢？如果不用编写和维护额外的配置代码也行，那何必还要它们呢。

大多数情况下，自动配置的Bean刚好能满足你的需要，不需要去覆盖它们。但某些情况情况下，Spring Boot在自动配置时的还不能很好地进行推断。

这里有个不错的例子：当你在应用程序里添加安全特性时，自动配置做得还不够好。安全配置并不是放之四海而皆准的，围绕应用程序安全有很多决策要做，Spring Boot不能替你做决定。虽然Spring Boot为安全提供了一些基本的自动配置，但是你还是需要自己覆盖一些配置以满足特定的安全要求。

想知道如何用显式的配置来覆盖自动配置，我们先从为阅读列表应用程序添加Spring Security入手。在了解自动配置提供了什么之后，我们再来覆盖基础的安全配置，以满足特定的场景需求。

### 3.1.1 保护应用程序

Spring Boot自动配置让应用程序的安全工作变得易如反掌，你要做的只是添加Security起步依赖。以Gradle为例，应添加如下依赖：
```
compile("org.springframework.boot:spring-boot-starter-security")
```
如果使用Maven，那么你要在项目的`<dependencies>`块中加入如下`<dependency>`：
```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

这样就搞定了！重新构建应用程序后运行即可，现在这就是一个安全的Web应用程序了！Security起步依赖在应用程序的Classpath里添加了Spring Secuirty（和其他一些东西）。Classpath里有Spring Security后，自动配置就能介入其中创建一个基本的Spring Security配置。

试着在浏览器里打开该应用程序，你马上就会看到HTTP基础身份验证对话框。此处的用户名是user，密码就有点麻烦了。密码是在应用程序每次运行时随机生成后写入日志的，你需要查找日志消息（默认写入标准输出），找到此类内容：

```
Using default security password: d9d8abe5-42b5-4f20-a32a-76ee3df658d9
```

我不能肯定，但我猜这个特定的安全配置并不是你的理想选择。首先，HTTP基础身份验证对话框有点粗糙，对用户并不友好。而且，我敢打赌你不会开发这种只有一个用户的应用程序，他还要从日志文件里找到自己的密码。因此，你会希望修改Spring Security的一些配置，至少要有一个好看一些的登录页，还要有一个基于数据库或轻量级访问协议（Lightweight Directory Access Protocol，LDAP）的身份验证服务。

让我们看看如何写出Spring Secuirty配置，覆盖自动配置的安全设置吧。

### 3.1.2 创建自定义的安全配置

覆盖自动配置很简单，就当自动配置不存在，直接显式地写一段配置。这段显式配置的形式不限，Spring支持的XML和Groovy形式配置都可以。

在编写显式配置时，我们会专注于Java形式的配置。在Spring Security的场景下，这意味着写一个扩展了`WebSecurityConfigurerAdapter`的配置类。代码清单3-1`SecurityConfig`就是我们需要的东西。

__代码清单3-1 覆盖自动配置的显式安全配置__

> 原书P51~52代码

>**文字**

>Require READER access ：要求登录者有READER角色

>Set login form path ：设置登录表单的路径

>Define custom UserDetailsService ：定义自定义`UserDetailsService`

`SecurityConfig`是个非常基础的Spring Security配置，尽管如此，它还是完成了不少安全定制工作。通过这个自定义的安全配置类，我们让Spring Boot跳过了安全自动配置，转而使用我们的安全配置。

扩展了`WebSecurityConfigurerAdapter`的配置类可以覆盖两个不同的`configure()`方法，在`SecurityConfig`里，第一个`configure()`方法指明“/”（`ReadingListController`的方法映射到了该路径）的请求只有经过身份认证且拥有READER角色的用户才能访问。其他的所有请求路径向所有用户开放了访问权限。这里还将登录页和登录失败页（带有一个`error`属性）指定到了/login。

Spring Security为身份认证提供了众多选项，后端可以是Java数据库连接（Java Database Connectivity，JDBC）、LDAP和内存用户存储。在这个应用程序中，我们会通过JPA用数据库来存储用户信息。第二个`configure()`方法设置了一个自定义的`UserDetailsService`，这个服务可以是任意实现了`UserDetailsService`的类，用于查找指定用户名的用户。代码清单3-2提供了一个匿名内部类实现，简单地调用了注入的`ReaderRepository`（这是一个Spring Data JPA仓库接口）的`findOne()`方法。

__代码清单3-2 用来持久化读者信息的仓库接口__

>原书P53代码

>**文字**

>Persist readers via JPA  通过JPA持久化读者

和`BookRepository`类似，你无需自己实现`ReaderRepository`，因为它扩展了`JpaRepository`，Spring Data JPA会在运行时自动创建它的实现，这为你提供了18个操作`Reader`实体的方法。

说到`Reader`实体，`Reader`类（如代码清单3-3所示）就是最后一块拼图了，它就是一个简单的JPA实体，其中有几个字段用来存储用户名、密码和用户全名。

__代码清单3-3 定义`Reader`的JPA实体__

>原书P53~54

>**文字**

>Reader fields ：Reader字段

>Grant READER privilege ：授予READER权限

>Do not expire, lock, or disable：不过期，不加锁，不禁用

如你所见，`Reader`用了`@Entity`注解，所以这是一个JPA实体。此外，它的`username`字段上有`@Id`注解，表明这是实体的ID。这个选择无可厚非，因为`username`应该能唯一标识一个`Reader`。

你应该还注意到`Reader`实现了`UserDetails`接口以及其中的方法，这样`Reader`就能代表Spring Security里的用户了。`getAuthorities()`方法被覆盖过了，始终会为用户授予READER权限。`isAccountNonExpired()`、 `isAccountNonLocked()`、`isCredentialsNonExpired()`和`isEnabled()`方法都返回`true`，这样读者账户就不会过期，不会被锁定，也不会被撤销。

重新构建并重启应用程序后，你应该就能以读者身份登录应用程序了。

> __保持简单__

> 在一个大型应用程序里，赋予用户的授权本身也可能是实体，它们被维护在独立的数据表里。同样，表示一个账户是否为非过期、非锁定且可用的布尔值也是数据库里的字段。但是，出于演示考虑，我决定让这些细节保持简单，不至于分散注意力，影响正在讨论的话题——我说的是覆盖Spring Boot自动配置。

在安全配置方面，我们还能做更多事情{![想要深入了解Spring Security，可以参考*Spring in Action*第4版（Manning，2014）中的第9章和第14章。]}，但此刻这样就足够了，上面的例子足以演示如何覆盖Spring Boot提供的安全自动配置。

再重申一次，想要覆盖Spring Boot的自动配置，你所要做的仅仅是编写一个显式的配置。Spring Boot会发现你的配置，随后降低自动配置的优先级，以你的配置为准。想弄明白这是如何实现的，让我们揭开Spring Boot自动配置的神秘面纱，看看它是如何运作的，它是怎么允许自己被覆盖的。

### 3.1.3 掀开自动配置的神秘面纱

正如我们在2.3.3节里讨论的那样，Spring Boot自动配置自带了很多配置类，每一个都能运用在你的应用程序里。它们都使用了Spring 4.0的条件化配置，可以在运行时判断这个配置是该被运用，还是该被忽略。

大部分情况下，表2-1里的`@ConditionalOnMissingBean`注解是覆盖自动配置的关键。Spring Boot的`DataSourceAutoConfiguration`中定义的`JdbcTemplate` Bean就是一个非常简单的例子，演示了`@ConditionalOnMissingBean`如何工作：
```
@Bean
@ConditionalOnMissingBean(JdbcOperations.class)
public JdbcTemplate jdbcTemplate() {
  return new JdbcTemplate(this.dataSource);
}
```
`jdbcTemplate()`方法上添加了`@Bean`注解，在需要时可以配置出一个`JdbcTemplate` Bean。但它上面还加了`@ConditionalOnMissingBean`注解，要求当前不存在`JdbcOperations`类型（`JdbcTemplate`实现了该接口）的Bean时才生效。如果当前已经有一个`JdbcOperations` Bean了，条件即不满足，不会执行`jdbcTemplate()`方法。

什么情况下会存在一个`JdbcOperations` Bean呢？Spring Boot的设计是加载应用级配置，随后再考虑自动配置类。因此，如果你已经配置了一个`JdbcTemplate` Bean，那么在执行自动配置时就已经存在一个`JdbcOperations`类型的Bean了，于是忽略自动配置的`JdbcTemplate` Bean。

关于Spring Security，自动配置会考虑几个配置类。在这里讨论每个配置类的细节是不切实际的，但覆盖Spring Boot自动配置的安全配置时，最重要的一个类是`SpringBootWebSecurityConfiguration`。以下是其中的一个代码片段：
```
@Configuration
@EnableConfigurationProperties
@ConditionalOnClass({ EnableWebSecurity.class })
@ConditionalOnMissingBean(WebSecurityConfiguration.class)
@ConditionalOnWebApplication
public class SpringBootWebSecurityConfiguration {

  ...

}
```
如你所见，`SpringBootWebSecurityConfiguration`上加了好几个注解。看到`@ConditionalOnClass`注解后，你就应该知道Classpath里必须要有`@EnableWebSecurity`注解。`@ConditionalOnWebApplication`说明这必须是个Web应用程序。`@ConditionalOnMissingBean`注解才是我们的安全配置类代替`SpringBootWebSecurityConfiguration`的关键所在。

`@ConditionalOnMissingBean`注解要求当下没有`WebSecurityConfiguration`类型的Bean。虽然表面上我们并没有这么一个Bean，但通过在`SecurityConfig`上添加`@EnableWebSecurity`注解，我们实际上间接创建了一个`WebSecurityConfiguration` Bean。所以在自动配置时，这个Bean就已经存在了，`@ConditionalOnMissingBean`条件不成立，`SpringBootWebSecurityConfiguration`提供的配置就被跳过了。

虽然Spring Boot的自动配置和`@ConditionalOnMissingBean`让你能显式地覆盖那些可以自动配置的Bean，但并不是每次都要做到这种程度。让我们来看看怎么通过设置几个简单的配置调整自动配置组件吧。

## 3.2 通过属性文件外置配置

在处理应用安全时，你当然会希望完全掌控所有配置。不过，为了微调一些细节，改改端口号和日志级别，便放弃自动配置，这是一件让人羞愧的事。设置数据库URL，配置一个属性简单，还是完整地声明一个数据源的Bean简单，答案不言而喻，不是吗？

事实上，Spring Boot自动配置的Bean提供了超过300个用于微调的属性。当你调整设置时，只要在环境变量、Java系统属性、JNDI（Java Naming and Directory Interface，命名目录服务）、命令行参数或者属性文件里进行指定就好了。

要了解这些属性，让我们来看个非常简单的例子。你也许已经注意到了，在命令行里运行阅读列表应用程序时，Spring Boot有一个ascii-art Banner。如果你想禁用这个Banner，可以将`spring.main.show-banner`属性设置为`false`，有几种实现方式，其中之一就是在运行应用程序的命令行参数里指定：
```
$ java -jar readinglist-0.0.1-SNAPSHOT.jar --spring.main.show-banner=false
```
另一种方式是创建一个名为application.properties的文件，包含如下内容：
```
spring.main.show-banner=false
```

或者，如果你喜欢的话，也可以创建名为application.yml的YAML文件，内容如下：
```
spring:
  main:
  show-banner: false
```
还可以将属性设置为环境变量。举例来说，如果你用的是bash或者zsh，可以用`export`命令：
```
$ export spring_main_show_banner=false
```
请注意，这里用的是下划线而不是点和横杠，这是对环境变量名称的要求。

实际上有好几种设置Spring Boot应用程序的途径，Spring Boot能从多种属性源获得属性，包括：

1. 命令行参数
2. `java:comp/env`里的JNDI属性
3. JVM系统属性
4. 操作系统环境变量
5. 随机生成的带random.*前缀的属性（在设置其他属性时，可以引用它们，比如`${random.long}`）
6. 应用程序以外的application.properties或者appliaction.yml文件
7. 打包在应用程序内的application.properties或者appliaction.yml文件
8. 通过`@PropertySource`标注的属性源
9. 默认属性

这个列表的按照优先级排序，也就是说，任何在高优先级属性源里设置的属性都会覆盖低优先级的相同属性。例如，命令行参数会覆盖其他属性源。

application.properties和application.yml文件能放在以下四个位置：

1. 外置，在相对于应用程序运行目录的/config子目录里。
2. 外置，在应用程序运行的目录里。
3. 内置，在config包内。
4. 内置，在Classpath根目录。

同样，这个列表按照优先级排序。也就是说/config子目录里的application.properties会覆盖应用程序Classpath里的application.properties中的相同属性。

此外，如果你在同一优先级位置同时有application.properties和application.yml，那么application.yml里的属性会覆盖application.properties里的属性。

禁用ascii-art Banner只是使用属性的小例子，再让我们看几个例子，如何通过常用途径微调自动配置的Bean。

### 3.2.1 自动配置微调

如我所说，有超过300个属性可以用来微调Spring Boot应用程序里的Bean。附录C有一个详尽的列表。此处无法逐一描述它们的细节，因此我们就通过几个例子来了解一些Spring Boot暴露的实用属性。

####1.禁用模板缓存

如果阅读列表应用程序经过了几番修改，你一定已经注意到了，除非重启应用程序，否则对Thymeleaf模板的变更是不会生效的。这是因为Thymeleaf模板默认缓存。这有助于改善应用程序的性能，因为模板只需编译一次，但在开发过程中就不能实时看到变更的效果。

将`spring.thymeleaf.cache`设置为`false`就能禁用Thymeleaf模板缓存。在命令行里运行应用程序时，将其设置为命令行参数即可：
```
$ java -jar readinglist-0.0.1-SNAPSHOT.jar --spring.thymeleaf.cache=false
```
或者，如果你希望每次运行时都禁用缓存，可以创建一个application.yml，包含以下内容：
```
spring:
  thymeleaf:
    cache: false
```
你一定要确保这个文件不会发布到生产环境，否则生产环境里的应用程序就无法享受模板缓存带来的福利了。

作为开发者，在修改模板时始终关闭缓存实在太方便了。为此，可以通过环境变量来禁用Thymeleaf缓存：
```
$ export spring_thymeleaf_cache=false
```
我们使用Thymeleaf作为应用程序的视图，Spring Boot支持的其他模板也能关闭模板缓存，设置这些属性就好了：

-  `spring.freemarker.cache`（Freemarker）
- `spring.groovy.template.cache`（Groovy模板）
- `spring.velocity.cache`（Velocity）

默认情况下，这些属性都为`true`，也就是开启缓存，将它们设置为`false`即可禁用缓存。

####2.配置嵌入式服务器

从命令行（或者Spring Tool Suite）运行Spring Boot应用程序时，应用程序会启动一个嵌入式的服务器（默认是Tomcat），监听8080端口。大部分情况下这挺好的，但同时运行多个应用程序可能会有问题。要是所有应用程序都试着让Tomcat服务器监听同一个端口，在启动第二个应用程序时就会有冲突。

不论出于什么原因，让服务器监听不同的端口，你所要做的就是设置`server.port`属性。要是只改一次，可以用命令行参数：
```
$ java -jar readinglist-0.0.1-SNAPSHOT.jar --server.port=8000
```
但如果希望端口变更时间更长一点，可以在其他支持的配置位置上设置`server.port`。例如，把它放在应用程序Classpath根目录的application.yml文件里：
```
server:
  port: 8000
```
除了服务器的端口，你还可能希望服务器提供HTTPS服务。为此，第一步就是用JDK的`keytool`工具来创建一个密钥存储（keystore）：
```
$ keytool -keystore mykeys.jks -genkey -alias tomcat -keyalg RSA
```
该工具会询问几个与名字和组织相关的问题，大部分都无关紧要。但在询问密码时，一定要记住你的选择。在本例中，我选择letmein作为密码。

现在只需要设置几个属性就能开启嵌入式服务器的HTTPS服务了。可以把它们都配置在命令行里，但这样太不方便了，可以把它们放在application.properties或application.yml里。在application.yml中，它们可能是这样的：
```
server:
  port: 8443
  ssl:
    key-store: file:///path/to/mykeys.jks
    key-store-password: letmein
    key-password: letmein
```
此处的`server.port`设置为8443，开发环境的HTTPS服务器大多会选这个端口。`server.ssl.key-store`属性指向密钥存储文件的存放路径，这里用了一个file://开头的URL，从文件系统里加载该文件。你也可以把它打在应用程序的JAR文件里，用`classpath: URL`来引用它。`server.ssl.key-store-password`和`server.ssl.key-password`设置为创建该文件时给定的密码。

有了这些属性，应用程序就能在8443端口上监听HTTPS请求了。（根据你所用的浏览器，可能会出现警告框提示该服务器无法验证其身份。在开发时，访问的是localhost，这没什么好担心的。）

####3.配置日志

你的应用程序不会直接记录日志，你所用的库也会记录它们的活动。

默认情况下，Spring Boot会用Logback（[http://logback.qos.ch](http://logback.qos.ch)）来记录日志，并用INFO级别输出到控制台。在运行应用程序和其他例子时，你应该已经看到很多INFO级别的日志了。

>__用其他日志实现替换Logback__

>一般来说，你不需要切换日志实现；Logback能很好地满足你的需要。但是，如果决定使用Log4j或者Log4j2，那么你只需要修改依赖，引入对应该日志实现的起步依赖，同时排除掉Logback。

>以Maven为例，应排除掉根起步依赖传递引入的默认日志起步依赖，这样就能排除Logback了：
>```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter</artifactId>
  <exclusions>
    <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```
>Gradle里，在`configurations`下排除该起步依赖是最简单的办法：
>```
configurations {
  all*.exclude group:'org.springframework.boot',
               module:'spring-boot-starter-logging'
}
```
>排除默认日志的起步依赖后，就可以引入你想用的日志实现的起步依赖了。在Maven里可以这样添加Log4j：
>```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-log4j</artifactId>
</dependency>
```
>在Gradle里可以这样添加Log4j：
>```
compile("org.springframework.boot:spring-boot-starter-log4j")
```
>如果你想用Log4j2，可以把spring-boot-starter-log4j改成spring-boot-starter-log4j2。

要完全掌握日志配置，可以在Classpath的根目录（src/main/resources）里创建logback.xml文件，下面是一个简单logback.xml的例子：
```
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>
        %d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
      </pattern>
    </encoder>
  </appender>

  <logger name="root" level="INFO"/>

  <root level="INFO">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```
除了日志格式之外，这个Logback配置和不加logback.xml文件的默认配置差不多。但是，通过编辑logback.xml你可以完全掌控应用程序的日志文件。哪些配置应该放进logback.xml这个话题不在本书的讨论范围内，请参考Logback的文档以了解更多信息。

即使如此，你最常改动的日志配置一般就是修改日志级别和指定日志输出的文件。使用了Spring Boot的配置属性后，你可以在不创建logback.xml文件的情况下修改那些配置。

要设置日志级别，你可以创建`logging.level`开头的属性，后面是你想设置日志级别的日志名称。举例来说，假设你要把根日志级别设置为`WARN`，但Spring Security的日志要用`DEBUG`级别。在application.yml里加入以下内容就行了：
```
logging:
  level:
    root: WARN
    org:
      springframework:
        security: DEBUG
```
另外，你也可以把Spring Security的包名写成一行：
```
logging:
  level:
    root: WARN
    org.springframework.security: DEBUG
```
现在，假设你想把日志写到位于/var/logs/目录里的BookWorm.log文件里。使用`logging.path`和`loggin.file`属性就行了：
```
logging:
  path: /var/logs/
  file: BookWorm.log
  level:
    root: WARN
    org:
      springframework:
        security: DEBUG
```
假设应用程序有/var/logs/的写权限，日志就能被写入/var/logs/BookWorm.log。默认情况下，日志文件的大小达到10MB时会切分一次。

与之类似，这些属性也能在application.properties里设置：
```
logging.path=/var/logs/
logging.file=BookWorm.log
logging.level.root=WARN
logging.level.root.org.springframework.security=DEBUG
```
如果你还是想要完全掌控日志配置，但是又不想用logback.xml作为Logback配置的名字，可以通过`logging.config`属性指定自定义的名字：
```
logging:
  config:
    classpath:logging-config.xml
```
虽然一般并不需要改变配置文件的名字，但是如果你想针对不同运行时Profile使用不同的日志配置（见3.2.3节），这个功能会很有用。

####4.配置数据源

此时，我们还在开发阅读列表应用程序，嵌入式的H2数据库能很好地满足我们的需要。可是一旦要投放到生产环境，我们可能要考虑更持久的数据库解决方案。

虽然你可以显式配置自己的`DataSource` Bean，但通常并不用这么做，只需简单地通过属性配置数据库的URL和身份信息就可以了。举例来说，如果你用的是MySQL数据库，你的application.yml文件看起来可能是这样的：
```
spring:
  datasource:
    url: jdbc:mysql://localhost/readinglist
    username: dbuser
    password: dbpass
```
通常你都无需指定JDBC驱动，Spring Boot会根据数据库URL识别出需要的驱动，但如果识别出问题了，你还可以设置`spring.datasource.driver-class-name`属性：
```
spring:
  datasource:
    url: jdbc:mysql://localhost/readinglist
    username: dbuser
    password: dbpass
    driver-class-name: com.mysql.jdbc.Driver
```
在自动配置`DataSource` Bean的时候，Spring Boot会使用这里的连接数据。`DataSource` Bean是一个连接池，如果Classpath里有Tomcat的连接池`DataSource`，那么就会使用这个连接池；否则，Spring Boot会在Classpath里查找以下连接池：

- HikariCP
- Commons DBCP
- Commons DBCP 2

这里列出的只是自动配置支持的连接池，你还可以自己配置`DataSource` Bean，使用你喜欢的各种连接池。

你也可以设置`spring.datasource.jndi-name`属性，从JNDI里查找`DataSource`：
```
spring:
  datasource:
    jndi-name: java:/comp/env/jdbc/readingListDS
```
一旦设置了`spring.datasource.jndi-name`属性，其他数据源连接属性都会被忽略，除非没有设置别的数据源连接属性。

有很多影响Spring Boot自动配置组件的方法，只需设置一两个属性即可。但这种配置外置的方法并不局限于Spring Boot配置的Bean，让我们看看如何使用这种属性配置机制来微调自己的应用程序组件。

### 3.2.2 应用程序Bean的配置外置

假设我们在某人的阅读列表里不止想要展示图书标题，还要提供该书的Amazon链接。不仅提供该书的链接，还要标记该书，利用好Amazon的Associate Program，这样如果有人用我们应用程序里的链接买了书，我们还能收到一笔推荐费。

这很简单，只需修改Thymeleaf模板，以链接的形式来呈现每本书的标题就可以了：

```
<a th:href="'http://www.amazon.com/gp/product/'
            + ${book.isbn}
            + '/tag=habuma-20'"
   th:text="${book.title}">Title</a>
```
这样就好了。现在如果有人点击该链接并购买了本书，我就能得到推荐费了，因为habuma-20是我的Amazon Associate ID。如果你也想收到推荐费，可以把Thymeleaf模板中tag的值改成你的Amazon Associate ID。

虽然在模板里修改这个值很简单，但毕竟这也算是硬编码。我们现在只在这一个模板里链接到Amazon，但后续可能会有更多页面要链接到Amazon，于是需要为应用程序添加功能。那样的话修改Amazon Associate ID就要改动好几个地方的代码。这就是为什么类似这种细节的东西最好不要放在代码里，要把它们集中在一个地方维护。

我们可以不在模板里硬编码Amazon Associate ID，而是把它变成模型中的一个值：
```
<a th:href="'http://www.amazon.com/gp/product/'
            + ${book.isbn}
            + '/tag=' + ${amazonID}"
   th:text="${book.title}">Title</a>
```  
此外，`ReadingListController`需要在模型里包含`amazonID`这个键，其中的内容是Amazon Associate ID。同样的道理，我们不应该硬编码这个值，而是应该引用一个实例变量，这个变量的值应该来自属性配置。代码清单3-4就是新的`ReadingListController`，它会返回注入的Amazon Associate ID。

__代码清单3-4 修改后的`ReadingListController`，能接受Amazon ID__

> 原书P65~66代码

>**文字**

> Inject with properties  ：属性注入

>Setter method for associateId  ：`associateId`的setter方法

>Put associateId into model  ：将`associateId`放入模型

如你所见，`ReadingListController`现在有了一个`associateId`属性，还有对应的`setAssociateId()`方法，用它可以设置该属性。`readersBooks()`现在能通过`amazonID`这个键把`associateId`的值放入模型。

棒极了！现在就剩一个问题——从哪里能取到`associateId`的值。

请注意，`ReadingListController`上加了`@ConfigurationProperties`注解，这说明该Bean的属性应该是（通过setter方法）从配置属性值注入的。说得更具体一点，`prefix`属性说明`ReadingListController`应该注入带amazon前缀的属性。

综合起来，我们指定`ReadingListController`的属性应该从amazon前缀的配置属性中进行注入。`ReadingListController`只有一个setter方法，就是设置`associateId`属性用的setter方法。因此，设置Amazon Associate ID唯一要做的就是添加`amazon.associateId`属性，把它加入支持的任一属性源位置里即可。

例如，我们可以在application.properties里设置该属性：
```
amazon.associateId=habuma-20
```
或者在application.yml里：
```
amazon:
  associateId: habuma-20
```
或者，我们可以将其设置为环境变量，把它作为命令行参数，把它加到任何能够设置配置属性的地方都可以。

> __开启配置属性__

> 从技术上来说，在向Spring配置类添加`@EnableConfigurationProperties`注解前，`@ConfigurationProperties`注解都不会生效。但通常无需这么做，因为Spring Boot自动配置后面的全部配置类都已经加上了`@EnableConfigurationProperties`注解。因此，除非你完全不使用自动配置（那怎么可能？），否则就无需显式地添加`@EnableConfigurationProperties`。

还有一点需要注意，Spring Boot的属性解析器非常智能，它会自动把驼峰规则的属性和使用连字符或下划线的同名属性关联起来。换句话说，`amazon.associateId`这个属性和`amazon.associate_id`以及`amazon.associate-id`都是等价的。用你习惯的命名规则就好。

####在一个类里收集属性

虽然在`ReadingListController`上加上`@ConfigurationProperties`注解跑起来没问题，但这并不是一个理想的方案。`ReadingListController`和Amazon没什么关系，但属性的前缀却是`amazon`，这看起来难道不奇怪么？再说，后续的各种功能可能需要在`ReadingListController`里新增配置属性，而它们和Amazon无关。

与其在`ReadingListController`里加载配置属性，还不如创建一个单独的Bean，为它加上`@ConfigurationProperties`属性，让这个Bean收集所有配置属性。代码清单3-5里的`AmazonProperties`就是一个例子，它用于加载Amazon相关的配置属性。

__代码清单3-5 在一个Bean里加载配置属性__

>原书P67

>**文字**

>Inject with “amazon”-prefixed properties：注入带amazon前缀的属性

>associateId setter method：associateId的setter方法

有了加载`amazon.associateId`配置属性的`AmazonProperties`后，我们可以调整`ReadingListController`（如代码清单3-6所示），让它从注入的`AmazonProperties`中获取Amazon Associate ID。

__代码清单3-6 注入了`AmazonProperties`的`ReadingListController`__

>原书P68

>**文字**

>Inject AmazonProperties：注入`AmazonProperties`

>Add Associate ID to model ：向模型中添加Associate ID

`ReadingListController`不再直接加载配置属性，转而通过注入其中的`AmazonProperties` Bean来获取所需的信息。

如你所见，配置属性在调优方面十分有用，这里说的调优不仅涵盖了自动配置的组件，还包括注入自有应用程序Bean的细节。但如果我们想为不同的部署环境配置不同的属性又该怎么办？让我们看看如何使用Spring的Profile来设置特定环境的配置。

### 3.2.3 使用Profile进行配置

当应用程序需要部署到不同的运行环境时，一些配置细节通常会有所不同。比如，数据库连接的细节在开发环境下和测试环境下就会不一样，在生产环境下又不一样。Spring Framework从Spring 3.1开始支持基于Profile的配置。Profile是一种条件化配置，基于运行时激活的Profile，会使用或者忽略不同的Bean或配置类。

举例来说，假设我们在代码清单3-1里创建的安全配置是针对生产环境的，而自动配置的安全配置用在开发环境就刚刚好。在这个例子中，我们就能为`SecurityConfig`加上`@Profile`注解：
```
@Profile("production")
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

...

}
```

这里用的`@Profile`注解要求运行时激活`production`Profile，这样才能应用该配置。如果`production`Profile没有激活，就会忽略该配置，此时缺少其他用于覆盖的安全配置，于是应用自动配置的安全配置。

设置`spring.profiles.active`属性就能激活Profile，任意设置配置属性的方式都能用于设置这个值。例如，在命令行里运行应用程序时，可以这样激活`production`Profile：
```
$ java -jar readinglist-0.0.1-SNAPSHOT.jar --
     spring.profiles.active=production
```
也可以向application.yml里添加`spring.profiles.active`属性：
```
spring:
  profiles:
    active: production
```
还可以设置环境变量，将其放入application.properties，3.2节开头时提到的各种方法都管用。

但Spring Boot的自动配置替你做了太多的事情，要找到一个能给你放`@Profile`的地方还真不怎么方便。

幸运的是，Spring Boot支持为application.properties和application.yml里的属性配置Profile。

为了演示区分Profile的属性，假设你希望针对生产环境和开发环境能有不同的日志配置。在生产环境中，你只关心WARN或更高级别的日志项，想把日志写到日志文件里。在开发环境中，你只想把日志打到控制台，记录DEBUG或更高级别。

而你所要做的就是为每个环境分别创建配置。那要怎么做呢？这取决于你用的是属性文件配置还是YAML配置。

####1.使用特定于Profile的属性文件

如果你正在使用application.properties，可以创建额外的属性文件，遵循application-{profile}.properties这种命名格式，这样就能提供特定于Profile的属性了。

在日志这个例子里，开发环境的配置可以放在名为application-development.properties的文件里，配置包含日志级别和输出到控制台：
```
logging.level.root=DEBUG
```
对于生产环境，application-production.properties会将日志级别设置为WARN或更高级别，并将日志写入日志文件：
```
logging.path=/var/logs/
logging.file=BookWorm.log
logging.level.root=WARN
```
与此同时，那些并不特定于哪个Profile或者保持默认值（以防万一有哪个特定于Profile的配置不指定这个值）的属性可以继续放在application.properties里：
```
amazon.associateId=habuma-20
logging.level.root=INFO
```

####2.使用多Profile YAML文件进行配置

如果你使用YAML来配置属性，可以遵循与配置文件相同的命名规范。即创建application-{profile}.yml这样的YAML文件，并将与Profile无关的属性继续放在application.yml里。

但既然用了YAML，你就可以把所有Profile的配置属性都放在一个application.yml文件里。举例来说，我们可以像下面这样声明日志配置：
```
logging:
  level:
    root: INFO

---

spring:
  profiles: development

logging:
  level:
    root: DEBUG

---

spring:
  profiles: production

logging:
  path: /tmp/
  file: BookWorm.log
  level:
    root: WARN
```
如你所见，这个application.yml文件分为三个部分，使用一组三个的连字符（`---`）作为分隔符。第二段和第三段分别为`spring.profiles`指定了一个值，这个值表示该部分配置应该应用在哪个Profile里。当中定义的属性应用于开发环境，因为`spring.profiles`设置为`development`。与之类似，最后一段的`spring.profile`设置为`production`，在`production` Profile被激活时生效。

另一方面，第一段并未指定`spring.profiles`，因此这里的属性对全部Profile都生效，或者对那些未设置该属性的激活Profile生效。

除了自动配置和外置配置属性，Spring Boot还有其他简化常用开发任务的绝招：它自动配置了一个错误页面，在应用程序遇到错误时显示。在结束本章内容之前，我们会看到Spring Boot的错误页，以及如何定制这个错误页来适应我们的应用程序。

## 3.3 定制应用程序错误页面

错误总是会发生的，那些在生产环境里鲁棒性最好的应用程序偶尔也会遇到麻烦。虽然减少用户遇到错误的概率很重要，但让应用程序展现一个好的错误页面也同样重要。

近些年来，富有创意的错误页已经成为了一种艺术。如果你曾见到过Github.com的星球大战错误页或者是Dropbox.com的Escher立方体错误页的话，你就能明白我在说什么了。

我不知道你在使用阅读列表应用程序时有没有碰到错误，如果有的话，你看到的页面应该和图3-1里的很像。

Spring Boot默认提供这个“白标”（whitelabel）错误页，这是自动配置的一部分。虽然这比错误跟踪栈要好一点，但和网上那些伟大的错误页艺术品却不可同日而语。为了让你的应用程序故障页变成大师级作品，你需要为应用程序创建一个自定义的错误页。

Spring Boot自动配置的默认错误处理器会查找名为error的视图，如果找不到就用默认的白标错误视图，如图3-1所示。因此，最简单的方法就是创建一个自定义视图，让解析出的视图名为error。

>P72图

__图3-1 Spring Boot的默认白标错误页面__

这一点归根到底取决于错误视图解析时的视图解析器，包括：

- 实现了Spring的`View`接口的Bean，其 ID为`ror`（由Spring的`BeanNameViewResolver`所解析）
- 如果配置了Thymeleaf，则是名为error.html的Thymeleaf模板
- 如果配置了FreeMarker，则是名为error.ftl的FreeMarker模板
- 如果配置了Velocity，则是名为error.vm的Velocity模板
- 如果是用JSP视图，则是名为error.jsp的JSP模板

因为我们的阅读列表应用程序使用了Thymeleaf，所以我们要做的就是创建一个名为error.html的文件，把它和其他的应用程序模板一起放在模板文件夹里。代码清单3-7是一个简单有效的错误页，可以用来代替默认的白标错误页。

__代码清单3-7 阅读列表应用程序的自定义错误页__

>原书P73

>**文字**

>Show requested path：显示请求路径

>Show error details：显示错误明细

这个自定义的错误模板应该命名为error.html，放在模板目录里，这样Thymeleaf模板解析器才能找到它。在典型的Maven或Gradle项目里，这就意味着要把该文件放在src/main/resources/templates中，运行时它就在Classpath的根目录里。

基本上，这个简单的Thymeleaf模板就是显示一张图片和一些提示错误的文字。其中有两处特别的信息需要呈现：错误的请求路径和异常消息。但这还不是错误页上的全部细节，默认情况下，Spring Boot会为错误视图提供如下错误属性：

- `timestamp`——错误发生的时间
- `status`——HTTP状态码
- `error`——错误原因
- `exception`——异常的类名
- `message`——异常消息（如果这个错误是由异常引起的）
- `errors`——`BindingResult`异常里的各种错误（如果这个错误是由异常引起的）
- `trace`——异常跟踪栈（如果这个错误是由异常引起的）
- `path`——错误发生时请求的URL路径

其中某些属性，比如`path`，在向用户交待问题时还是很有用的。其他的，比如`trace`，用起来要保守一点，将其隐藏或者是用得聪明点，让错误页尽可能对用户友好。

请注意，模板里还引用了一张名为MissingPage.png的图片。图片的实际内容并不重要，所以尽情挑选适合你的图片就好了，但请一定将它放在src/main/resources/static或src/main/resources/public里，这样应用程序运行时才能找到它。

图3-2是发生错误时用户会看到的页面。虽然它算不上一件艺术品，但还是把应用程序错误页的艺术水准稍微提高了那么一点。

>-P74图

__图3-2 遇到错误时展现的自定义错误页__

## 3.4 小结

Spring Boot消除了Spring应用程序中经常要用到的很多样板式配置。让Spring Boot处理全部配置，你可以仰仗它来配置那些适合你应用程序的组件。当自动配置无法满足需求时，Spring Boot允许你覆盖并微调它提供的配置。

覆盖自动配置其实很简单，就是显式地编写那些没有Spring Boot时你要做的Spring配置。Spring Boot的自动配置被设计为优先使用应用程序提供的配置，然后才轮到自己的自动配置。

即使自动配置合适，你仍然需要调整一些细节。Spring Boot会开启多个属性解析器，让你通过环境变量、属性文件、YAML文件等多种方式来设置属性，以此微调配置。这套基于属性的配置模型也能用于应用程序自己定义的组件，可以从外部配置源加载属性并注入到Bean里。

Spring Boot还自动配置了一个简单的白标错误页，虽然它比异常和跟踪栈要更友好一点，但在艺术性方面这个错误页还有很大的提升空间。幸运的是Spring Boot提供了好几种选项来自定义或完全替换这个白标错误页，让它能满足应用程序的特定风格。

现在我们已经用Spring Boot写了一个完整的应用程序，我们会验证它能否满足预期。除了自己在浏览器里手工点点之外，我们还应该要写一些自动化、可重复运行的测试来检查这个应用程序，证明它能正确运作。这也是我们在第4章里要做的事。
