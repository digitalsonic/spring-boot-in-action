# 第2章 开发第一个应用程序

> 本章内容

>- 使用Spring Boot起步依赖
- 自动进行Spring配置

你上次在超市或大型零售店自己推开门是什么时候？大多数大型商店都安装了带感应功能的自动门，虽然所有门都能让你进入建筑物内，但自动门不用你动手推拉。

与之类似，很多公共场所的卫生间里都装有自动水龙头和纸巾售卖机。虽然没有超市自动门这么普及，但这些设施同样对你没有太多要求，可以很方便地出水和纸巾。

说实话，我已经不记得上次看到制冰盒是什么时候了，更记不得自己往里面倒水制冰或者取冰的事了。我的冰箱就是这么神奇，总是有加冰的水能让我喝上一杯。

我敢打赌你也能想出无数例子，说明设备让现代生活更自动化，而不是增加障碍。有了这些自动化的便利设施，你一定会希望在开发任务里看到更多类似的东西。但是很奇怪，事实并非如此。

直到最近，用Spring创建应用程序还要求你为框架做很多事情。当然，Spring提供了很多优秀的特性，用于开发令人惊讶的应用程序。但是，你需要自己往项目的构建说明文件里添加各种库依赖；你还要自己写配置文件，告诉Spring做什么。

Spring Boot将Spring开发的自动化程度提升到了一个新的高度，在本章我们会看到两种新方法：起步依赖和自动配置。在项目中启用Spring不仅枯燥乏味，还让人抓狂，你将看到这些基础的Spring Boot特性是如何将你解放出来的，它能让你集中精力开发自己的应用程序。与此同时，你会写一个很小的Spring应用程序，麻雀虽小，五脏俱全，其中会用上Spring Boot。

## 2.1 运用Spring Boot

你正在阅读本书，说明你是一位读书人。也许你就是一个书虫，博览群书；也可能你只读自己需要的东西，拿起本书只是为了知道怎么用Spring开发应用程序。

无论何种情况，你都是一位读书人，是读书人便有心维护一个阅读列表，里面是自己想读（或者需要读）的书。就算没有白纸黑字的列表，至少在你心里会有这么一个列表。{![要是你不是一个读书人，就把书换成想看的电影、想去的餐厅，只要合适自己就好。]}

在本书中，我们会构建一个简单的阅读列表应用程序，在这个程序里，用户可以输入想读的图书信息，查看列表，删除已经读过的书。我们将使用Spring Boot来辅助快速开发，各种繁文缛节越少越好。

开始前，我们需要先初始化一个项目。在第1章里，我们看到了好几种从Spring Initializr开始Spring Boot开发的方法，因为选择哪种都行，所以要选个最合适的，着手用Spring Boot开发就好了。

从技术角度来看，我们要用Spring MVC来处理Web请求，用Thymeleaf来定义Web视图，用Spring Data JPA来把阅读列表持久化到数据库里，姑且先用嵌入式的H2数据库。虽然也可以用Groovy，但是我们还是先用Java来开发这个应用程序吧。此外，我们使用Gradle作为构建工具。

无论是用Web界面、Spring Tool Suite还是IntelliJ IDEA，只要是用了Initializr，你就要确保勾选了Web、Thymeleaf和JPA这几个复选框，还要记得勾上H2复选框，这样才能在开发应用程序时使用这个内嵌式数据库。

至于项目元信息，就随便你写了。以阅读列表为例，我创建项目时使用了图2-1中的信息。

>P25图

__图2-1 通过Initializr的Web界面初始化阅读列表应用程序__

如果你创建项目时用的是Spring Tool Suite或者IntelliJ IDEA，那么把图2-1的内容适配成IDE需要的东西就好了。

另一方面，如果用Spring Boot CLI来初始化应用程序，可以在命令行里键入以下内容：
```
$ spring init -dweb,data-jpa,h2,thymeleaf --build gradle readinglist
```
请记住，CLI的`init`命令是不能指定项目根包名和项目名的，包名默认是demo，项目名默认是Demo。在项目创建完毕之后，你可以打开项目，把包名demo改为readinglist，把DemoApplication.java改名为ReadingListApplication.java。

项目创建完毕后，你应该能看到一个类似图2-2的项目结构。

>P26图

__图2-2 初始化后的readinglist项目结构__

这个项目结构基本上和第1章里Initializr生成的结构是一样的，只不过你现在是真的要去开发应用程序了，让我们先放慢脚步，仔细看看初始化出来的项目里都有什么东西。

### 2.1.1 查看初始化的Spring Boot新项目

图2-2中值得注意的第一件事是，整个项目结构遵循传统Maven或Gradle项目的布局，即主要应用程序代码位于src/main/java目录里，资源都在src/main/resources目录里，测试代码则在src/test/java目录里。此刻还没有测试资源，但如果有的话要放在src/test/resources里。

再进一步，你会看到项目里还有不少文件。

- build.gradle：Gradle构建说明文件。
- `ReadingListApplication.java`：应用程序的启动引导类（bootstrap class），也是主要的Spring配置类。
- `application.properties`：用于配置应用程序和Spring Boot的属性。
- `ReadingListApplicationTests.java`：一个基本的集成测试类。

因为构建说明文件里有很多Spring Boot的优点尚未揭秘，所以我打算把最好的留到最后，先让我们来看看`ReadingListApplication.java`。

####1.启动引导Spring

`ReadingListApplication`在Spring Boot应用程序里有两个作用：配置和启动引导。首先，这是主要的Spring配置类，虽然Spring Boot的自动配置免除了很多Spring配置，但你还需要进行少量配置来启用自动配置。正如代码清单2-1所示，这里只有一行配置代码。

__代码清单2-1 `ReadingListApplication.java`不仅是启动引导类，还是配置类__

>原书P27代码

>**文字**

>Enable component-scanning and auto-configuration-开启组件扫描和自动配置

>Bootstrap the applicantion-负责启动引导应用程序

`@SpringBootApplication`开启了Spring的组件扫描和Spring Boot的自动配置功能。实际上，`@SpringBootApplication`将三个有用的注解组合在了一起。

- *Spring*的`@Configuration`：标明该类使用Spring基于Java的配置。虽然本书不会写太多配置，但我们会更倾向于使用基于Java而不是XML的配置。
- *Spring*的`@ComponentScan`：启用组件扫描，这样你写的Web控制器类和其他组件才能被自动发现并注册为Spring应用程序上下文里的Bean。本章稍后会写一个简单的Spring MVC控制器，使用`@Controller`进行注解，这样组件扫描才能找到它。
- *Spring Boot* 的`@EnableAutoConfiguration`：这个不起眼的小注解也可以称为`@Abracadabra`{![abracadabra的意思是咒语。——译者注]}，就是这一行配置开启了Spring Boot自动配置的魔力，让你不用再写成篇的配置了。

在Spring Boot的早期版本中，你需要在`ReadingListApplication`类上同时标上这三个注解，但从Spring Boot 1.2.0开始，有`@SpringBootApplication`就行了。

如我所说，`ReadingListApplication`还是一个启动引导类。要运行Spring Boot应用程序有几种方式，其中包含传统的WAR文件部署。但这里的`main()`方法让你可以在命令行里把该应用程序当作一个可执行JAR文件来运行。这里向`SpringApplication.run()`里传递了一个`ReadingListApplication`类的引用，还有命令行参数，通过这些东西启动应用程序。

实际上，就算一行代码也没写，此时你仍然可以构建应用程序尝尝鲜，要构建并运行应用程序，最简单的方法就是用Gradle的`bootRun`任务：
```
$ gradle bootRun
```
`bootRun`任务来自Spring Boot的Gradle插件，我们会在2.1.2节里详细讨论。此外，你也可以用Gradle构建项目，然后在命令行里用`java`来运行它：
```
$ gradle build
...
$ java -jar build/libs/readinglist-0.0.1-SNAPSHOT.jar
```
应用程序应该能正常运行，启动一个监听8080端口的Tomcat服务器。要是愿意，你可以用浏览器访问[http://localhost:8080](http://localhost:8080)，但由于还没写控制器类，你只会收到一个HTTP 404 （Not Found）错误，看到错误页面。在本章结束前，这个URL将会提供一个阅读列表应用程序。

你几乎不需要修改`ReadingListApplication.java`，如果你的应用程序需要Spring Boot自动配置以外的其他Spring配置，一般来说，最好把它写到一个`@Configuration`标注的单独类里。（组件扫描会发现这些类的。）极度简单的情况下，可以把自定义配置加入`ReadingListApplication.java`。

####2.测试Spring Boot应用程序

Initializr还提供了一个测试类的骨架，可以基于它为你的应用程序编写测试。但`ReadingListApplicationTests`（代码清单2-2）不止是个用于测试的占位符，它还是一个例子，告诉你如何为Spring Boot应用程序编写测试。

**代码清单2-2 `@SpringApplicationConfiguration`加载Spring应用程序上下文**

>原书P28代码

>**文字**

>Load context via Spring Boot ：通过Spring Boot加载上下文

>Test that the context loads ：测试加载的上下文

一个典型的Spring集成测试会用`@ContextConfiguration`注解标识如何加载Spring的应用程序上下文。但是，为了充分发挥Spring Boot的魔力，这里应该用`@SpringApplicationConfiguration`注解。正如你在代码清单2-2里看到的那样，`ReadingListApplicationTests`使用`@SpringApplicationConfiguration`注解从`ReadingListApplication`配置类里加载Spring应用程序上下文。

`ReadingListApplicationTests`里还有一个简单的测试方法，即`contextLoads()`。实际上它就是个空方法。但这个空方法足以证明应用程序上下文的加载没有问题。如果`ReadingListApplication`里定义的配置是好的，就能通过测试。如果有问题，测试就会失败。

当然，现在这只是一个新的应用程序，你还会添加自己的测试。但`contextLoads()`方法是个良好的开端，此刻可以验证应用程序提供的各种功能。第4章里我们会更详细地讨论如何测试Spring Boot应用程序。

####3.配置应用程序属性

Initializr为你生成的application.properties文件是一个空文件。实际上，这个文件完全是可选的，你大可以删掉它，这不会对应用程序有任何影响，但留着也没什么问题。

稍后，我们肯定有机会向application.properties里添加几个条目。但现在，如果你想小试牛刀，可以加一行看看：
```
server.port=8000
```
加上这一行，嵌入式Tomcat的监听端口就变成了8000，而不是默认的8080。你可以重新运行应用程序看看是不是这样。

这说明application.properties文件可以很方便地帮你细粒度地调整Spring Boot的自动配置。你还可以用它来指定应用程序代码所需的配置项。在第3章里我们会看到好几个例子，演示`application.properties`的这两种用法。

要注意的是，你完全不用告诉Spring Boot为你加载`application.properties`，只要它存在就会被加载，Spring和应用程序代码都能获取其中的属性。

我们差不多已经把初始化的项目介绍完了，还剩最后一样东西，让我们来看看Spring Boot应用程序是如何构建的。

### 2.1.2 Spring Boot项目构建过程解析

Spring Boot应用程序大部分内容都与其他Spring应用程序没有什么区别，其实其他Java应用程序也没什么两样，构建一个Spring Boot应用程序和构建其他Java应用程序的过程类似。你可以选择Gradle或Maven作为构建工具，描述构建说明文件的方法和描述非Spring Boot应用程序的方法相似。但是，Spring Boot在构建过程中耍了些小把戏，在此需要做个小小的说明。

Spring Boot为Gradle和Maven提供了构建插件，以便辅助构建Spring Boot项目。代码清单2-3是Initializr创建的build.gradle文件，其中应用了Spring Boot的Gradle插件。

__代码清单2-3 使用Spring Boot的Gradle插件__

>原书P30

>**文字**

>Depend on Spring Boot plugin ：依赖Spring Boot插件

>Apply Spring Boot plugin ：运用Spring Boot插件

>Starter dependencies ：起步依赖

另一方面，要是选择用Maven来构建你的应用程序，Initializr会替你生成一个pom.xml文件，其中使用了Spring Boot的Maven插件，如代码清单2-4所示。

__代码清单2-4 使用Spring Boot的Maven插件及父起步依赖__

>原书P31代码

>**文字**

>Inherit versions from starter parent  ：从spring-boot-starter-parent继承版本号

>Starter dependencies ：起步依赖

>Apply Spring Boot plugin：  运用Spring Boot插件

无论你选择Gradle还是Maven，Spring Boot的构建插件都对构建过程有所帮助。你已经看到过如何用Gradle的`bootRun`任务来运行应用程序了，Spring Boot的Maven插件与之类似，提供了一个`spring-boot:run`目标，如果你使用Maven，它能实现相同的功能。

构建插件的主要功能是把项目打包成一个可执行的超级JAR（uber-JAR），包括把应用程序的所有依赖打入JAR文件内，并为JAR添加一个描述文件，其中的内容能让你用`java -jar`来运行应用程序。

除了构建插件，代码清单2-4里的Maven构建说明中还将spring-boot-starter-parent作为上一级，这样一来就能利用Maven的依赖管理功能，继承很多常用库的依赖版本，在你声明依赖时就不用再去指定版本号了。请注意，这个pom.xml里的`<dependency>`都没有指定版本。

遗憾的是，Gradle并没有Maven这样的依赖管理功能，为此Spring Boot Gradle插件提供了第三个特性，它为很多常用的Spring及其相关依赖模拟了依赖管理功能。其结果就是，代码清单2-3的build.gradle里也没有为各项依赖指定版本。

说起依赖，无论哪个构建说明文件，都只有五个依赖，除了你手工添加的H2之外，其他的Artifact ID都有spring-boot-starter-前缀。这些都是Spring Boot起步依赖，它们都有助于Spring Boot应用程序的构建。让我们来看看它们究竟都有哪些好处。

##2.2 使用起步依赖

要理解Spring Boot起步依赖带来的好处，先让我们假设它们尚不存在。如果没用Spring Boot的话，你会向项目里添加哪些依赖呢？要用Spring MVC的话，你需要哪个Spring依赖？你还记得Thymeleaf的Group和Artifact ID么？你应该用哪个版本的Spring Data JPA呢？它们放在一起兼容么？

噢，看来如果没有Spring Boot起步依赖，你就有不少作业要做。而你想要做的只不过是开发一个Spring Web应用程序，使用Thymeleaf视图，通过JPA进行数据持久化。但在开始编写第一行代码之前，你得搞明白，要支持你的计划，需要往构建说明里加入哪些东西。

考虑再三之后（也许你还从其他有相似依赖的应用程序构建说明中复制粘贴了不少内容），你的Gradle构建说明里大概会有下面这些东西：
```
compile("org.springframework:spring-web:4.1.6.RELEASE")
compile("org.thymeleaf:thymeleaf-spring4:2.1.4.RELEASE")
compile("org.springframework.data:spring-data-jpa:1.8.0.RELEASE")
compile("org.hibernate:hibernate-entitymanager:jar:4.3.8.Final")
compile("com.h2database:h2:1.4.187")
```
这段依赖列表不错，应该能正常工作，但你是怎么知道的？你怎么保证你选的这些版本能相互兼容？也许可以，但构建并运行应用程序之前你是不知道的。再说了，你怎么知道这个列表是完整的？一行代码都没写的情况下，你离开始构建还有很长的路要走。

让我们退一步再想想，我们要做什么。我们要构建一个拥有如下功能的应用程序。

- 这是一个Web应用程序。
- 它用了Thymeleaf。
- 它通过Spring Data JPA在关系型数据库里持久化数。

如果我们只在构建文件里指定这些功能，让构建过程自己搞明白我们要什么东西，岂不是好？这正是Spring Boot起步依赖的功能。

### 2.2.1 指定基于功能的依赖

Spring Boot通过提供众多起步依赖降低项目依赖的复杂度。起步依赖本质上来说是一个Maven项目对象模型（Project Object Model，POM），定义了对其他库的传递依赖，这些东西加在一起即支持某项功能。很多起步依赖的命名都暗示了它们提供的某种或某类功能。

举例来说，你打算把这个阅读列表应用程序做成一个Web应用程序，与其向项目的构建文件里添加一堆单独的库依赖，还不如声明这是一个Web应用程序来得简单。你只要添加Spring Boot的Web起步依赖就好了。

我们还想以Thymeleaf为Web视图，用JPA来实现数据持久化，因此在构建文件里还需要Thymeleaf和Spring Data JPA的起步依赖。

为了能进行测试，我们还需要能在Spring Boot上下文里运行集成测试的库，因此要添加Spring Boot的test起步依赖，这是一个测试时依赖。

统统放在一起，我们就有了这五个依赖，也就是Initializr在Gradle的构建文件里提供的：
```
dependencies {
  compile "org.springframework.boot:spring-boot-starter-web"
  compile "org.springframework.boot:spring-boot-starter-thymeleaf"
  compile "org.springframework.boot:spring-boot-starter-data-jpa"
  compile "com.h2database:h2"
  testCompile("org.springframework.boot:spring-boot-starter-test")
}
```
正如先前所见，添加这些依赖的最快方法就是在Initializr里选中Web、Thymeleaf和JPA复选框。但如果在初始化项目时没有这么做，当然也可以稍后再编辑生成的build.gradle或pom.xml。

通过传递依赖，添加这四个依赖就等价于加了一大把独立的库，这些传递依赖涵盖了Spring MVC、Spring Data JPA、Thymeleaf等内容，它们声明的依赖也会被传递依赖进来。

最值得注意的是，这四个起步依赖的具体程度恰到好处。我们并没有说想要Spring MVC，只是说想要构建一个Web应用程序。我们并没有指定JUnit或其他测试工具，只是说我们想要测试自己的代码。Thymeleaf和Spring Data JPA的起步依赖稍微具体一点，但这也只是由于没有更模糊的方法声明这种需要。

这四个起步依赖只是Spring Boot众多起步依赖中的沧海一粟。附录B罗列出了全部起步依赖，并稍微描述了一下它们向项目构建引入了什么。

我们并不需要指定版本号，起步依赖本身的版本是由正在使用的Spring Boot的版本来决定的，而起步依赖则会决定它们引入的传递依赖的版本。

不知道自己所用依赖的版本，你多少会有些不安，你要有信心，相信Spring Boot经过了足够的测试，确保引入的全部依赖都能相互兼容。这是一种解脱，只需指定起步依赖，不用担心自己需要维护哪些库，也不必担心它们的版本。

但如果你真想知道自己在用什么，在构建工具里总能找到你要的答案。在Gradle里，`dependencies`任务会显示一个依赖树，其中包含了项目所用的每一个库以及它们的版本：
```
$ gradle dependencies
```
在Maven里使用`dependency`插件的`tree`目标也能获得相似的依赖树。
```
$ mvn dependency:tree
```
大部分情况下，你都无需关心每个Spring Boot起步依赖分别声明了些什么东西。Web起步依赖能让你构建Web应用程序，Thymeleaf起步依赖能让你用Thymeleaf模板，Spring Data JPA起步依赖能让你用Spring Data JPA将数据持久化到数据库里，通常只要知道这些就足够了。

但是，即使经过了Spring Boot团队的测试，起步依赖里所选的库仍有问题该怎么办？如何覆盖起步依赖呢？

### 2.2.2 覆盖起步依赖引入的传递依赖

说到底，起步依赖和你项目里的其他依赖没什么区别。也就是说，你可以通过构建工具中的功能，选择性地覆盖它们引入的传递依赖的版本号，排除传递依赖，当然还可以为那些Spring Boot起步依赖没有涵盖的库指定依赖。

以Spring Boot的Web起步依赖为例，它传递依赖了Jackson JSON库，如果你正在构建一个生产或消费JSON资源表述的REST服务，那它会很有用。但是，要构建传统的面向人类用户的Web应用程序，你可能用不上Jackson。虽然把它加进来也不会有什么坏处，但排除掉它的传递依赖，可以为你的项目瘦身。

如果在用Gradle，你可以这样排除传递依赖：
```
compile("org.springframework.boot:spring-boot-starter-web") {
  exclude group: 'com.fasterxml.jackson.core'
}
```
在Maven里，可以用`<exclusions>`元素来排除传递依赖，下面这个引入Spring Boot的build.gradle的`<dependency>`增加了`<exclusions>`元素去除Jackson：
```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <groupId>com.fasterxml.jackson.core</groupId>
    </exclusion>
  </exclusions>
</dependency>
```
另一方面，也许项目需要Jackson，但你需要用另一个版本的Jackson来进行构建，而不是Web起步依赖里的那个。假设Web起步依赖引用了Jackson 2.3.4，但你需要使用2.4.3{![此处提到的版本仅作演示之用，Spring Boot的Web起步依赖所引用的实际Jackson版本由你使用的Spring Boot版本决定。]}，在Maven里，你可以直接在pom.xml中表达诉求，就像这样：
```
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.4.3</version>
</dependency>
```
Maven总是会用最近的依赖，也就是说，你在项目的构建说明文件里增加的这个依赖，会覆盖传递依赖引入的另一个依赖。

与之类似，如果你用的是Gradle，可以在build.gradle文件里指明你要的Jackson的版本：
```
compile("com.fasterxml.jackson.core:jackson-databind:2.4.3")
```
因为这个依赖的版本比Spring Boot的Web起步依赖引入的要新，所以在Gradle里是生效的。但假如你要的不是新版本的Jackson，而是一个较早的版本呢？Gradle和Maven不太一样，Gradle倾向于使用库的最新版本。因此，如果你要使用老版本的Jackon，则不得不把老版本的依赖加入构建，并把Web起步依赖传递依赖的那个版本排除掉：

```
compile("org.springframework.boot:spring-boot-starter-web") {
  exclude group: 'com.fasterxml.jackson.core'
}
compile("com.fasterxml.jackson.core:jackson-databind:2.3.1")
```
不管什么情况，在覆盖Spring Boot起步依赖依引入的传递依赖时都要多加小心。虽然不同的版本放在一起也许没什么问题，但你要知道起步依赖中各个依赖版本之间的兼容性都经过了精心的测试。应该只在特殊的情况下覆盖这些传递依赖（比如新版本修复了一个bug）。

现在我们有了一个空的项目结构，构建说明文件也准备好了，是时候开发应用程序了。我们会让Spring Boot来处理配置细节，而我们自己则专注于编写阅读列表功能相关的代码。

## 2.3 使用自动配置

简而言之，Spring Boot的自动配置是一个运行时（更准确地说，是应用程序启动时）的过程，考虑了众多因素，才决定Spring配置应该用哪个，不该用哪个。举几个例子，下面这些情况都是Spring Boot的自动配置要考虑的：

- Spring的`JdbcTemplate`是不是在Classpath里？如果是的话，有没有`DataSource`的Bean呢，有的话就自动配置一个`JdbcTemplate`的Bean。
- Thymeleaf是不是在Classpath里？如果是的话，配置Thymeleaf的模板解析器、视图解析器以及模板引擎。
- Spring Security是不是在Classpath里？如果是的话，做一个非常基本的Web安全设置。

每当应用程序启动的时候，Spring Boot的自动配置都要做将近200个这样的决定，涵盖安全、集成、持久化、Web开发等诸多方面。所有这些自动配置就是为了尽量不让你自己写配置。

有意思的是，自动配置的东西很难写在书本里，如果不能写出配置，那又该怎么描述并讨论它们呢？

### 2.3.1 专注于应用程序功能

要为Spring Boot的自动配置博得好感，我可以在接下来的几页里向你演示没有Spring Boot的情况下需要写哪些配置。但眼下已经有不少好书写过这些内容了，再写一次并不能让我们更快地写好阅读列表应用程序。

既然知道Spring Boot会替我们料理这些事情，那么与其浪费时间讨论这些Spring配置，还不如看看如何利用Spring Boot的自动配置，让我们能专注于应用程序代码。除了开始写代码，我想不到更好的办法了。

####1.定义领域模型

我们应用程序里的核心领域概念是读者阅读列表上的书。因此我们需要定义一个实体类来表示这个概念，代码清单2-5演示了如何定义一本书。

__代码2-5 表示列表里的书的`Book`类__
```
package readinglist;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Book {

  @Id
  @GeneratedValue(strategy=GenerationType.AUTO)
  private Long id;
  private String reader;
  private String isbn;
  private String title;
  private String author;
  private String description;

  public Long getId() {
    return id;
  }

  public void setId(Long id) {
    this.id = id;
  }

  public String getReader() {
    return reader;
  }

  public void setReader(String reader) {
    this.reader = reader;
  }

  public String getIsbn() {
    return isbn;
  }

  public void setIsbn(String isbn) {
    this.isbn = isbn;
  }

  public String getTitle() {
    return title;
  }

  public void setTitle(String title) {
    this.title = title;
  }

  public String getAuthor() {
    return author;
  }

  public void setAuthor(String author) {
    this.author = author;
  }

  public String getDescription() {
    return description;
  }

  public void setDescription(String description) {
    this.description = description;
  }
}
```
如你所见，`Book`类就是简单的Java对象，其中有些描述书的属性，还有必要的访问方法。`@Entity`注解表明它是一个JPA实体，`id`属性加了`@Id`和`@GeneratedValue`注解，说明这个字段是实体的唯一标识，并且这个字段的值是自动生成的。

####2.定义仓库接口

接下来，我们就要定义用于把`Book`对象持久化到数据库的仓库了。{![原文这里写的是`ReadingList`对象，但文中并没有定义这个对象，看代码应该是`Book`对象。——译者注]}因为用了Spring Data JPA，所以我们要做的就是简单地定义一个接口，扩展一下Spring Data JPA的`JpaRepository`接口：
```
package readinglist;

import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ReadingListRepository extends JpaRepository<Book, Long> {
  List<Book> findByReader(String reader);
}
```

通过扩展`JpaRepository`，`ReadingListRepository`有18个执行常用持久化操作的方法直接继承。`JpaRepository`是个泛型接口，有两个参数：仓库操作的领域对象类型，及其ID属性的类型。此外，我还增加了一个`findByReader()`方法，可以根据读者的用户名来查找阅读列表。

如果你好奇谁来实现这个`ReadingListRepository`及其集成的18个方法，请不用担心，Spring Data提供了很神奇的魔法，只需定义仓库接口，在应用程序启动后，该接口在运行时会自动实现。

####3.创建Web界面

现在，我们定义好了应用程序的领域模型，还有把领域对象持久化到数据库里的仓库接口，剩下的就是创建Web前端了。代码清单2-6的Spring MVC控制器就能为应用程序处理HTTP请求。

__代码清单2-6 作为阅读列表应用程序前端的Spring MVC控制器__

```
package readinglist;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import java.util.List;

@Controller
@RequestMapping("/")
public class ReadingListController {
  private ReadingListRepository readingListRepository;

  @Autowired
  public ReadingListController(
             ReadingListRepository readingListRepository) {
    this.readingListRepository = readingListRepository;
  }

  @RequestMapping(value="/{reader}", method=RequestMethod.GET)
  public String readersBooks(
      @PathVariable("reader") String reader,
      Model model) {
    List<Book> readingList =
        readingListRepository.findByReader(reader);
    if (readingList != null) {
      model.addAttribute("books", readingList);
    }
    return "readingList";
  }

  @RequestMapping(value="/{reader}", method=RequestMethod.POST)
  public String addToReadingList(
            @PathVariable("reader") String reader, Book book) {
    book.setReader(reader);
    readingListRepository.save(book);
    return "redirect:/{reader}";
  }
}
```
`ReadingListController`使用了`@Controller`注解，这样组件扫描会自动将其注册为Spring应用程序上下文里的一个Bean，它还用了`@RequestMapping`注解，将其中所有的处理器方法都映射到了“/”这个URL路径上。

该控制器有两个方法：

- `readersBooks()`：处理`/{reader}`上的HTTP `GET`请求，根据路径里指定的读者，从（通过控制器的构造器注入的）仓库获取`Book`列表。随后将这个列表塞入模型，用的键是`books`，最后返回`readingList`作为呈现模型的视图逻辑名称。
- `addToReadingList()`：处理`/{reader}`上的HTTP `POST`请求，将请求正文里的数据绑定到一个`Book`对象上，该方法把`Book`对象的`reader`属性设置为读者的姓名，随后通过仓库的`save()`方法保存修改后的`Book`对象，最后重定向到`/{reader}`（控制器中的另一个方法会处理该请求）。

`readersBooks()`方法最后返回`readingList`作为逻辑视图名，为此必须创建该视图。因为在项目开始之初我就决定要用Thymeleaf来定义应用程序的视图，所以接下来就在src/main/resources/templates里创建一个名为readingList.html的文件，内容如代码清单2-7所示：

__代码清单2-7 呈现阅读列表的Thymeleaf模板__
```
<html>
  <head>
    <title>Reading List</title>
    <link rel="stylesheet" th:href="@{/style.css}"></link>
  </head>
  <body>
    <h2>Your Reading List</h2>
    <div th:unless="${#lists.isEmpty(books)}">
      <dl th:each="book : ${books}">
        <dt class="bookHeadline">
          <span th:text="${book.title}">Title</span> by
          <span th:text="${book.author}">Author</span>
          (ISBN: <span th:text="${book.isbn}">ISBN</span>)
        </dt>
        <dd class="bookDescription">
          <span th:if="${book.description}"
                th:text="${book.description}">Description</span>
          <span th:if="${book.description eq null}">
                No description available</span>
        </dd>
      </dl>
    </div>
    <div th:if="${#lists.isEmpty(books)}">
      <p>You have no books in your book list</p>
    </div>

    <hr/>

    <h3>Add a book</h3>
    <form method="POST">
      <label for="title">Title:</label>
        <input type="text" name="title" size="50"></input><br/>
      <label for="author">Author:</label>
        <input type="text" name="author" size="50"></input><br/>
      <label for="isbn">ISBN:</label>
        <input type="text" name="isbn" size="15"></input><br/>
      <label for="description">Description:</label><br/>
        <textarea name="description" cols="80" rows="5">
        </textarea><br/>
      <input type="submit"></input>
    </form>

  </body>
</html>
```

这个模板定义了一个HTML页面，该页面概念上分为两个部分；页面上方是读者的阅读列表中的图书清单，下方是是一个表单，读者可以从这里添加新书。

为了美观，Thymeleaf模板引用了一个名为style.css的样式文件，该文件位于src/main/resources/static目录中，看起来是这样的：

```
body {
    background-color: #cccccc;
    font-family: arial,helvetica,sans-serif;
}
.bookHeadline {
    font-size: 12pt;
    font-weight: bold;
}
.bookDescription {
    font-size: 10pt;
}
label {
    font-weight: bold;
}
```

这个样式表并不复杂，也没有过分追求让应用程序变漂亮，但已经能满足我们的需求了，很快你就会看到，它能用来演示Spring Boot的自动配置功能。

不管你相不相信，以上就是一个完整的应用程序了——本章已经向你呈现了所有的代码。等一下，回顾一下前几页的内容，你看到什么配置了吗？实际上，除了代码清单2-1里的三行配置（这是开启自动配置所必需的），你不用再写任何Spring配置了。

虽然没什么Spring配置，但是这已经是一个可以运行的完整Spring应用程序了。让我们把它跑起来，看看是怎么样的。

###2.3.2 运行应用程序

运行Spring Boot应用程序有几种方法。先前在2.5节里，我们讨论了如何通过Maven和Gradle来运行应用程序，以及如何构建并运行可执行JAR。稍后，在第8章里你将看到如何构建WAR文件，并用传统的方式部署到Java Web应用服务器里，比如Tomcat。

假设你正使用Spring Tool Suite开发应用程序，可以在IDE里选中项目，在Run菜单里选择Run As > Spring Boot App，通过这种方式来运行应用程序，如图2-3所示。

> P43图

__图2-3 在Spring Tool Suite里运行Spring Boot应用程序__

如果一切正常，你的浏览器应该会展现一个空白的阅读列表，下方有一个用于向列表添加新书的表单，如图2-4所示。接下来，通过表单添加一些图书吧，随后你的阅读列表看起来就会像图2-5这样。

>P44图2.4

__图2-4 初始状态下的空阅读列表__

>P44图2.5

__图2-5 添加了一些图书后的阅读列表__

再多用用这个应用程序吧，你准备好之后，我们就来看一下Spring Boot是如何做到不写Spring配置代码就能开发整个Spring应用程序的。

###2.3.3 刚刚发生了什么

如我所说，在没有配置代码的情况下，很难描述自动配置。与其花时间讨论那些你不用做的事情，不如在这一节里关注一下你要做的事——写代码。

当然，某处肯定是有些配置的。配置是Spring Framework的核心元素，必须要有东西告诉Spring如何运行应用程序。

在向应用程序加入Spring Boot时，有个名为spring-boot-autoconfigure的JAR文件，其中包含了很多配置类。每个配置类都在应用程序的Classpath里，都有机会为应用程序的配置添砖加瓦。这些配置类里有用于Thymeleaf的配置，有用于Spring Data JPA的配置，有用于Spiring MVC的配置，还有很多其他东西的配置，你可以自己选择是否在Spring应用程序里使用它们。

所有这些配置如此与众不同，原因在于它们利用了Spring的条件化配置，这是Spring 4.0引入的新特性。条件化配置允许配置存在于应用程序中，但在满足某些特定条件之前都忽略这个配置。

在Spring里可以很方便地编写你自己的条件，你所要做的就是实现`Condition`接口，覆盖它的`matches()`方法。举例来说，下面这个简单的条件类只有在Classpath里存在`JdbcTemplate`时才会生效：
```java
package readinglist;
import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.type.AnnotatedTypeMetadata;

public class JdbcTemplateCondition implements Condition {
  @Override
  public boolean matches(ConditionContext context,
                         AnnotatedTypeMetadata metadata) {
    try {
      context.getClassLoader().loadClass(
             "org.springframework.jdbc.core.JdbcTemplate");
      return true;
    } catch (Exception e) {
      return false;
    }
  }
}
```
当你用Java来声明Bean的时候，可以使用这个自定义条件类：
```java
@Conditional(JdbcTemplateCondition.class)
public MyService myService() {
    ...
}
```
在这个例子里，只有当`JdbcTemplateCondition`类的条件成立时才会创建`MyService`这个Bean。也就是说`MyService` Bean创建的条件是Classpath里有`JdbcTemplate`。否则，这个Bean的声明就会被忽略掉。

虽然本例中的条件相当简单，但Spring Boot定义了很多更有趣的条件，并把它们运用到了配置类上，这些配置类构成了Spring Boot的自动配置。Spring Boot运用条件化配置的方法是，定义多个特殊的条件化注解，并将它们用到配置类上。表2-1列出了Spring Boot提供的条件化注解。

__表2-1 自动配置中使用的条件化注解__

| 条件化注解                       | 配置生效条件                                                            |
|---------------------------------|-------------------------------------------------------------------------|
| `@ConditionalOnBean`              | 配置了某个特定Bean                                                       |
| `@ConditionalOnMissingBean`       | 没有配置特定的Bean                                                       |
| `@ConditionalOnClass`             | Classpath里有指定的类                                                    |
| `@ConditionalOnMissingClass`      | Classpath里缺少指定的类                                                  |
| `@ConditionalOnExpression`        | 给定的Spring Expression Language（SpEL）表达式计算结果为`true`             |
| `@ConditionalOnJava`              | Java的版本匹配特定值或者一个范围值                                         |
| `@ConditionalOnJndi`              | 参数中给定的JNDI位置必须存在一个，如果没有给参数，则要有JNDI `InitialContext` |
| `@ConditionalOnProperty`          | 指定的配置属性要有一个明确的值                                             |
| `@ConditionalOnResource`          | Classpath里有指定的资源                                                   |
| `@ConditionalOnWebApplication`    | 这是一个Web应用程序                                                       |
| `@ConditionalOnNotWebApplication` | 这不是一个Web应用程序                                                     |

一般来说，Spring Boot自动配置类的源代码无需查看，但为了演示表2-1里的注解如何使用，我们可以看一下`DataSourceAutoConfiguration`里的这个片段（这是Spring Boot自动配置库的一部分）：
```
@Configuration
@ConditionalOnClass({ DataSource.class, EmbeddedDatabaseType.class })
@EnableConfigurationProperties(DataSourceProperties.class)
@Import({ Registrar.class, DataSourcePoolMetadataProvidersConfiguration.class })
public class DataSourceAutoConfiguration {
...
}
```

如你所见，`DataSourceAutoConfiguration`添加了`@Configuration`注解，它从其他配置类里导入了一些额外配置，还自己定义了一些Bean。最重要的是，`DataSourceAutoConfiguration`上添加了`@ConditionalOnClass`注解，要求Classpath里必须要有`DataSource`和`EmbeddedDatabaseType`。如果它们不存在，条件就不成立，`DataSourceAutoConfiguration`提供的配置都会被忽略掉。

`DataSourceAutoConfiguration`里嵌入了一个`JdbcTemplateConfiguration`类，自动配置了一个`JdbcTemplate` Bean：
```
@Configuration
@Conditional(DataSourceAutoConfiguration.DataSourceAvailableCondition.class)
protected static class JdbcTemplateConfiguration {

  @Autowired(required = false)
  private DataSource dataSource;

  @Bean
  @ConditionalOnMissingBean(JdbcOperations.class)
  public JdbcTemplate jdbcTemplate() {
    return new JdbcTemplate(this.dataSource);
  }

...

}
```
`JdbcTemplateConfiguration`使用了`@Conditional`注解，判断`DataSourceAvailableCondition`条件是否成立——基本上就是要有一个`DataSource` Bean或者要自动配置创建一个。假设有`DataSource` Bean，使用了`@Bean`注解的`jdbcTemplate()`方法会配置一个`JdbcTemplate` Bean。这个方法上还加了`@ConditionalOnMissingBean`注解，因此只有在不存在`JdbcOperations`类型（即`JdbcTemplate`实现的接口）的Bean时才会创建`JdbcTemplate` Bean。

此处看到的只是`DataSourceAutoConfiguration`的冰山一角，Spring Boot提供的其他自动配置类也有很多知识没有提到。但这已经足以说明Spring Boot如何利用条件化配置实现自动配置。

自动配置会做出以下配置决策，它们和之前的例子息息相关：

- 因为Classpath里有H2，所以会有一个嵌入式的H2数据库Bean创建，它的类型是`javax.sql.DataSource`，JPA实现（Hibernate）需要它来访问数据库。
- 因为Classpath里有Hibernate的（Spring Data JPA传递引入的）实体管理器，所以自动配置会配置与Hibernate相关的Bean，包括Spring的`LocalContainerEntityManagerFactoryBean`和`JpaVendorAdapter`。
- 因为Classpath里有Spring Data JPA，所以会自动配置为根据仓库的接口创建仓库实现。
- 因为Classpath里有Thymeleaf，所以Thymeleaf会配置为Spring MVC的视图，包括一个Thymeleaf的模板解析器、模板引擎及视图解析器。视图解析器会解析相对于Classpath根目录的/templates目录里的模板。
- 因为Classpath里有Spring MVC（归功于Web起步依赖），所以会配置Spring的`DispatcherServlet`并启用Spring MVC。
- 因为这是一个Spring MVC Web应用程序，所以会注册一个资源处理器，把相对于Classpath根目录的/static目录里的静态内容提供出来。（这个资源处理器还能处理/public、/resources和/META-INF/resources的静态内容）。
- 因为Classpath里有Tomcat（通过Web起步依赖传递引用），所以会启动一个嵌入式的Tomcat容器，监听8080端口。

由此可见，Spring Boot自动配置承担起了配置Spring的重任，因此你能专注于编写自己的应用程序。

## 2.4 小结

通过Spring Boot的起步依赖和自动配置，你可以更加快速、便捷地开发Spring应用程序。起步依赖帮助你专注于应用程序需要的功能类型，而非提供该功能的具体库和版本。与此同时，自动配置把你从样板式的配置中解放了出来，这些配置在没有Spring Boot的Spring应用程序里非常常见。

虽然自动配置很方便，但在开发Spring应用程序时其中的一些用法也有点武断。要是你在配置Spring时希望或者需要有所不同该怎么办？在第3章，我们将会看到如何覆盖Spring Boot自动配置，藉此达成应用程序的一些目标，还有如何运用类似的技术来配置自己的应用程序组件。
