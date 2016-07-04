# 第5章 Groovy与Spring Boot CLI

>本章内容

>- 自动依赖与`import`
- 获取依赖
- 测试基于CLI的应用程序

有些东西真的很适合在一起：花生酱和果酱，Abbott和Costello***{![两位都是美国的喜剧演员，详见[https://en.wikipedia.org/wiki/Abbott_and_Costello](https://en.wikipedia.org/wiki/Abbott_and_Costello)。——译者注]}***，电闪和雷鸣，牛奶和饼干。每样东西都很棒，但搭配起来就更赞了。

到目前为止，我们已经看到了Spring Boot带来的不少好东西，包括自动配置和起步依赖。要是再搭配上Groovy的优雅，就能起到一加一大于二的效果。

在本章中，我们会了解Spring Boot CLI。这是一个命令行工具，将强大的Spring Boot和Groovy结合到一起，针对Spring应用程序形成了一套简单而又强大的开发工具。为了演示Spring Boot CLI的强大之处，我们会回到第2章的阅读列表应用程序，利用CLI的优势，以Groovy重写这个应用程序。

## 5.1 开发Spring Boot CLI应用程序

大部分针对JVM平台的项目都用Java语言开发，引入了诸如Maven或Gradle这样的构建系统，以生成可部署的产物。实际上，我们在第2章开发的阅读列表应用程序就遵循这套模型。

最近版本的Java语言有不少改进。然而，即便如此，Java还是有一些严格的规则为代码增加了不少噪声。行尾分号、类和方法的修饰符（比如`public`和`private`）、getter和setter方法，还有`import`语句在Java中都有自己的作用，但它们同代码的本质无关，因而造成了干扰。从开发者的角度来看，代码噪声是阻力——编写代码时是阻力，试图阅读代码时更是阻力。如果能消除一部分代码噪声，代码的开发和阅读可以更加方便。

同理，Maven和Gradle这样的构建系统在项目中也有自己的作用，但你还得为此开发和维护构建说明。如果能直接构建，项目也会更加简单。

在使用Spring Boot CLI时，没有构建说明文件。代码本身就是构建说明，提供线索指引CLI解析依赖，并生成用于部署的产物。此外，配合Groovy，Spring Boot CLI提供了一种开发模型，消除了几乎所有代码噪声，带来了畅通无阻的开发体验。

在最简单的情况下，编写基于CLI的应用程序就和编写第1章里的那个Groovy脚本一样简单。不过，要用CLI编写更完整的应用程序，就需要设置一个基本的项目结构来容纳项目代码。我们马上用它重写阅读列表应用程序。

### 5.1.1 设置CLI项目

我们要做的第一件事是创建目录结构，容纳项目。与基于Maven或Gradle的项目不同，Spring Boot CLI项目并没有严格的项目结构要求。实际上，最简单的Spring Boot CLI应用程序就是一个Groovy脚本，可以放在文件系统的任意目录里。对阅读列表应用程序而言，你应该创建一个干净的新目录来存放代码，把它们和你电脑上的其他东西分开。
```
$ mkdir readinglist
```
此处我将目录命名为readinglist，但你可以随意命名。比起找个地方放置代码，名字并不重要。

我们还需要两个额外的目录存放静态Web内容和Thymeleaf模板。在readinglist目录里创建两个新的目录，名为static和templates。
```
$ cd readinglist
$ mkdir static
$ mkdir templates
```
这些目录的名字和基于Java的项目中src/main/resources里的目录同名。虽然Spring Boot并不像Maven和Gradle那样，对目录结构有严格的要求，但Spring Boot会自动配置一个Spring `ResourceHttpRequestHandler`查找static目录（还有其他位置）的静态内容。还会配置Thymeleaf来解析templates目录里的模板。

说到静态内容和Thymeleaf模板，那些文件的内容和我们在第2章里创建的一样。因此你不用担心稍后需要回忆它们，直接把style.css复制到static目录，把readingList.html复制到templates目录。

此时，阅读列表项目的目录结构应该是这样的：

>原书P94，离5.1.2标题最近的代码

现在项目已经设置好了，我们准备好编写Groovy代码了。

### 5.1.2 通过Groovy消除代码噪声

Groovy本身是种优雅的语言。与Java不同，Groovy并不要求有`public`和`private`这样的限定符，也不要求在行尾有分号。此外，归功于Groovy的简化属性语法（GroovyBeans），JavaBean的标准访问方法没有存在的必要了。

随之而来的结果是，用Groovy编写`Book`领域类相当简单。如果在阅读列表项目的根目录里创建一个新的文件，名为Book.groovy，那么在这里编写如下Groovy类。
```
class Book {
    Long id
    String reader
    String isbn
    String title
    String author
    String description
}
```
如你所见，Groovy类与它的Java类相比，大小完全不在一个量级。这里没有setter和getter方法，没有`public`和`private`修饰符，也没有分号。Java中常见的代码噪声不复存在，剩下的内容都在描述书的基本信息。

> __Spring Boot CLI中的JDBC与JPA__

>你也许已经注意到了，`Book`的Groovy实现与第2章里的Java实现有所不同，上面没有添加JPA注解。这是因为这里要用Spring的`JdbcTemplate`，而非Spring Data JPA访问数据库。

> 有好几个不错的理由能解释这个例子为什么选择JDBC而非JPA。首先，在使用Spring的`JdbcTemplate`时，我可以多用几种不同的方法，展示Spring Boot的更多自动配置技巧。选择JDBC的最主要原因是，Spring Data JPA在生成仓库接口的自动实现时要求有一个.class文件。当你在命令行里运行Groovy脚本时，CLI会在内存里编译脚本，并不会产生.class文件。因此，当你在CLI里运行脚本时，Spring Data JPA并不适用。

>但CLI和Spring Data JPA并非完全不兼容。如果使用CLI的`jar`命令把应用程序打包成一个JAR文件，结果文件里就会包含所有Groovy脚本编译后的.class文件。当你想部署一个用CLI开发的应用程序时，在CLI里构建并运行JAR文件是一个不错的选择。但是如果你想在开发时快速看到开发内容的效果，这种做法就没那么方便了。

既然我们定义好了`Book`领域类，就开始编写仓库接口吧。首先，编写`ReadingListRepository`接口（位于ReadingListRepository.groovy）：
```
interface ReadingListRepository {

    List<Book> findByReader(String reader)

    void save(Book book)

}
```

除了没有分号，以及接口上没有`public`修饰符，`ReadingListRepository`的Groovy版本和与之对应的Java版本并无二致。最显著的区别是它没有扩展`JpaRepository`。本章我们不用Spring Data JPA，既然如此，我们就不得不自己实现`ReadingListRepository`。**代码清单5-1**就是JdbcReadingListRepository.groovy的内容。

__代码清单5-1 `ReadingListRepository`的Groovy JDBC实现__

>原书P95~96代码

>文字

>Inject JdbcTemplate=注入`JdbcTemplate`

>RowMapper closure=`RowMapper`闭包

以上代码的大部分内容在实现一个典型的基于`JdbcTemplate`的仓库。它自动注入了一个`JdbcTemplate`对象的引用，用它查询数据库获取图书（在`findByReader()`方法里），将图书保存到数据库（在`save()`方法里）。

因为编写过程采用了Groovy，所以我们在实现中可以使用一些Groovy的语法糖。举个例子，在`findByReader()`里，调用`query()`时可以在需要`RowMapper`实现的地方传入一个Groovy闭包。***{![为了公平对待Java，在Java 8里我们可以用Lambda（和方法引用）做类似的事情。]}***此外，闭包中创建了一个新的`Book`对象，在构造时设置对象的属性。

在考虑数据库持久化时，我们还需要创建一个名为schema.sql的文件。其中包含创建`Book`表所需的SQL。仓库在发起查询时依赖这个数据表：
```
create table Book (
        id identity,
        reader varchar(20) not null,
        isbn varchar(10) not null,
        title varchar(50) not null,
        author varchar(50) not null,
        description varchar(2000) not null
);
```

稍后我会解释如何使用schema.sql。现在你只需要知道，把它放在Classpath的根目录（即项目的根目录），就能创建出查询用的`Book`表了。

Groovy的所有部分差不多都齐全了，但还有一个Groovy类必须要写。这样Groovy化的阅读列表应用程序才完整。我们需要编写一个`ReadingListController`的Groovy实现来处理Web请求，为浏览器提供阅读列表。在项目的根目录，要创建一个名为ReadingListController.groovy的文件，内容如代码清单5-2所示。

__代码清单5-2 处理展示和添加Web请求的`ReadingListController`__

>原书P97代码

>文字：

>Inject ReadingListRepository：注入`ReadingListRepository`

>Fetch reading list：获取阅读列表

>Populate model：设置模型

>Return view name：返回视图名称

>Save book：保存图书

>Redirect after POST：`POST`后重定向

这个`ReadingListController`和第2章里的版本有很多相似之处。主要的不同在于，Groovy的语法消除了类和方法的修饰符、分号、访问方法和其他不必要的代码噪声。

你还会注意到，两个处理器方法都用`def`而非`String`来定义。两者都没有显式的`return`语句。如果你喜欢在方法上说明类型，喜欢显式的`retrun`语句，加上就好了——Groovy并不在意这些细节。

在运行应用程序之前，还要做一件事。那就是创建一个新文件，名为Grabs.groovy，内容包括如下三行：

```
@Grab("h2")
@Grab("spring-boot-starter-thymeleaf")
class Grabs {}
```

稍后我们再来讨论这个类的作用，现在你只需要知道类上的`@Grab`注解会告诉Groovy在启动应用程序时自动获取一些依赖的库。

不管你信还是不信，我们已经可以运行这个应用程序了。我们创建了一个项目目录，向其中复制了一个样式表和Thymeleaf模板，填充了一些Groovy代码。接下来，用Spring Boot CLI（在项目目录里）运行即可：

```
$ spring run .
```

几秒后，应用程序完全启动。打开浏览器，访问[http://localhost:8080](https://localhost:8080)。如果一切正常，你应该就能看到和第2章一样的阅读列表应用程序。

成功啦！只用了几页纸的篇幅，你就写出了简单而又完整的Spring应用程序！

此时此刻你也许会好奇这是怎么办到的。

- *没有Spring配置*，Bean是如何创建并组装的？`JdbcTemplate` Bean又是从哪来的？
- *没有构建文件*，Spring MVC和Thymeleaf这样的依赖库是哪来的？
- *没有`import`语句*。如果不通过`import`语句来指定具体的包，Groovy如何解析`JdbcTemplate`和`RequestMapping`的类型？
- *没有部署应用*，Web服务器从何而来？

实际上，我们编写的代码看起来不止缺少分号。这些代码究竟是怎么运行起来的？

### 5.1.3 发生了什么

你可能已经猜到了，Spring Boot CLI在这里不仅仅是便捷地使用Groovy编写了Spring应用程序。Spring Boot CLI施展了很多技能。

* CLI可以利用Spring Boot的自动配置和起步依赖。
* CLI可以检测到正在使用的特定类，自动解析合适的依赖库来支持那些类。
* CLI知道多数常用类都在哪些包里，如果用到了这些类，它会把那些包加入Groovy的默认包里。
* 应用自动依赖解析和自动配置后，CLI可以检测到当前运行的是一个Web应用程序，并自动引入嵌入式Web容器（默认是Tomcat）供应用程序使用。

仔细想想，这些才是CLI提供的最重要的特性。Groovy语法只是额外的福利！

通过Spring Boot CLI运行阅读列表应用程序，表面看似平凡无奇，实则大有乾坤。CLI尝试用内嵌的Groovy编译器来编译Groovy代码。虽然你不知道，但实际上，未知类型（比如`JdbcTemplate`、`Controller`及`RequestMapping`，等等）最终会使代码编译失败。

但CLI不会放弃，它知道只要把Spring Boot JDBC起步依赖加入Classpath就能找到`JdbcTemplate`。它还知道把Spring Boot的Web起步依赖加入Classpath就能找到Spring MVC的相关类。因此，CLI会从Maven仓库（默认为Maven中心仓库）里获取那些依赖。

如果此时CLI重新编译，那还是会失败，因为缺少`import`语句。但CLI知道很多常用类的包。利用定制Groovy编译器默认包导入的功能之后，CLI把所有需要用到的包都加入了Groovy编译器的默认导入列表。

现在CLI可以尝试再一次编译了。假设没有其他CLI能力范围外的问题（比如，存在CLI不知道的语法或类型错误），代码就能完成编译。CLI将通过内置的启动方法（与基于Java的例子里的`main()`方法类似）运行应用程序。

此时，Spring Boot自动配置就能发挥作用了。它发现Classpath里存在Spring MVC（因为CLI解析了Web起步依赖），就自动配置了合适的Bean来支持Spring MVC，还有嵌入式Tomcat Bean供应用程序使用。它还发现Classpath里有`JdbcTemplate`，所以自动创建了`JdbcTemplate` Bean，注入了同样自动创建的`DataSource` Bean。

说起`DataSource` Bean，这只是Spring Boot自动配置创建的众多Bean中的一个。Spring Boot还自动配置了很多Bean来支持Spring MVC中的Thymeleaf模板。正是由于我们使用`@Grab`注解向Classpath里添加了H2和Thymeleaf，这才触发了针对嵌入式H2数据库和Thymeleaf的自动配置。

`@Grab`注解的作用是方便添加CLI无法自动解析的依赖。虽然它看上去很简单，但实际上这个小小的注解作用远比你想象得要大。让我们仔细看看这个注解，看看Spring Boot CLI是如何通过一个Artifact名称找到这么多常用依赖，看看整个依赖解析的过程是如何配置的。

## 5.2 获取依赖

在Spring MVC和`JdbcTemplate`的例子中，为了获取必要的依赖并添加到Classpath里，Groovy编译触发了Spring Boot CLI。这是错误的。但如果需要一个依赖，而没有失败代码来触发自动依赖解析，又或者所需的依赖CLI不知道，那该怎么办？

在阅读列表应用程序中，我们需要Thymeleaf库，这样才能编写使用了Thymeleaf模板的视图。我们还需要H2的库，这样才能拥有嵌入式的H2数据库。但因为没有Groovy代码会直接引用Thymeleaf或H2的类，所以不会有编译错误来触发自动依赖解析。因此，我们要帮一帮CLI，在`Grabs`类上添加`@Grab`依赖。

> __该把`@Grab`注解放在哪里？__

> 并不需要像我们这样，严格将`@Grab`注解放在一个单独的类上。它们在`ReadingListController`或`JdbcReadingListRepository`同样有效。不过，为了便于组织管理，最好创建一个空类，把所有`@Grab`注解放在一起。这样方便在一个地方看到所有显式声明的依赖。

`@Grab`注解源自Groovy Grape（Groovy Adaptable Packaging Engine或Groovy Advanced Packaging Engine）工具。从本质上来说，Grape允许Groovy脚本在运行时下载依赖，无需Maven或Gradle这样的构建工具介入。除了支持`@Grab`注解，Spring Boot CLI还用Grape来获取代码中推断出的依赖。

使用`@Grab`就和描述依赖一样简单。举例来说，假设你想往项目里添加H2数据库，可以往项目的一个Groovy脚本添加如下`@Grab`注解：
```
@Grab(group="com.h2database", module="h2", version="1.4.190")
```
这样能明确地声明依赖的组、模块和版本号。或者，你也可以用更简洁的冒号分割表示依赖，这和Gradle构建说明里的表示方式类似。
```
@Grab("com.h2database:h2:1.4.185")
```
这是两个教科书式的例子，但Spring Boot CLI对`@Grab`做了几处扩展，用起来更简单。

很多依赖不再要求指定版本号了。可以通过下面的方式，用`@Grab`添加H2数据库依赖：
```
@Grab("com.h2database:h2")
```
确切的版本号是由你所使用的CLI的版本来决定的。如果用的是Spring Boot CLI 1.3.0.RELEASE，那么H2依赖的版本会解析为1.4.190。

这还不算完，很多常用依赖还可以省去Group ID，直接在`@Grab`里写上模块的ID。正是这个特性让上文的`@Grab`注解成功加载了H2。
```
@Grab("h2")
```
那你该如何获知某个依赖是需要Group ID和版本号，还是只需要Module ID呢？我在附录D中提供了一个完整的列表，包含了Spring Boot CLI知道的全部依赖。通常，你可以先试一下只写Module ID，如果这样不行，再加上Group ID和版本号。

只用Module ID来表示依赖会很方便，但如果你并不认可Spring Boot选择的版本号怎么办？如果Spring Boot的起步依赖传递引入了一个库的某个版本，但你想要使用修正了bug的新版本又该如何呢？

### 5.2.1 覆盖默认依赖版本

Spring Boot引入了新的`@GrabMetadata`注解，可以和`@Grab`搭配使用，用属性文件里的内容来覆盖默认的依赖版本。

要用`@GrabMetadata`，可以把它加到某个Groovy脚本文件里，提供相应的属性文件来覆盖依赖元数据：
```
@GrabMetadata(“com.myorg:custom-versions:1.0.0”)
```  
这会从Maven仓库的com/myorg目录里加载一个名为custom-versions.properties的文件。文件里的每一行都应该有Group ID和Module ID。以这两个东西为键名，属性则是值。例如，要把H2的默认版本覆盖为1.4.186，可以把`@GrabMetadata`指向一个包含如下内容的属性文件：
```
com.h2database:h2=1.4.186
```
> __使用Spring IO平台__

> 你可能希望让`@GrabMetadata`使用Spring IO平台（[http://platform.spring.io/platform/](http://platform.spring.io/platform/)）上定义的依赖版本。该平台提供了一套依赖和版本。明确哪个版本的Spring能和其他库的什么版本搭配使用。Spring IO平台提供的依赖和版本是Spring Boot已知依赖库的一个超集，包含了很多Spring应用程序经常用到的第三方库。

>   如果你想在Spring IO平台上构建Spring Boot CLI应用程序，只需要在Groovy脚本中添加如下`@GrabMetadata`即可。
>```
@GrabMetadata('io.spring.platform:platform-versions:1.0.4.RELEASE')
```
>  这会覆盖CLI的默认依赖版本，使Spring IO平台定义的版本取而代之。

你可能会有疑问，Grape又是从哪里获取所有这些依赖的呢？这是可配置的吗？让我们来看看你该如何管理Grape获取依赖的仓库集。

### 5.2.2 添加依赖仓库

默认情况下，`@Grab`声明的依赖是从Maven中心仓库（[http://repo1.maven.org/maven2/](http://repo1.maven.org/maven2/)）拉取的。此外，Spring Boot还注册了Spring的里程碑及快照仓库，以便获取Spring项目的预发布版本依赖。对很多项目而言，这就足够了。但要是你的项目需要的库不在这两者之中该怎么办呢？或者你的工作环境在公司防火墙内，必须使用内部仓库又该如何？

没有问题。`@GrabResolver`注解可以让你指定额外的仓库，用来获取依赖。

举个例子，假设你想使用最新的Hibernate，而最新的Hibernate版本只能从JBoss的仓库里获取到。那么你需要通过`@GrabResolver`来添加仓库：
```
@GrabResolver(name='jboss', root=
  'https://repository.jboss.org/nexus/content/groups/public-jboss')
```
这里通过`name`属性将该解析器命名为jboss，通过`root`属性来指定仓库的URL。

你已经了解了Spring Boot CLI是如何编译代码以及自动按需解析已知依赖库的。在`@Grab`的支持下，CLI可以解析各种它无法自动解析的依赖。基于CLI的应用程序无需Maven或Gradle构建说明文件（传统方式开发的Java应用程序需要这个文件）。但解析依赖和编译代码并不是构建过程的全部，项目的构建通常还要执行自动化测试，要是没有构建说明文件，又该如何运行测试呢？

## 5.3 用CLI运行测试

测试是软件项目的重要组成部分，Spring Boot CLI当然没有忽略测试。因为基于CLI的应用程序并未涉及传统的构建系统，所以CLI提供了一个`test`命令来运行测试。

在试验`test`命令前，你先要写一个测试。测试可以放在项目中的任何位置。我建议将其与主要组件分开放置，最好放在一个子目录里。这个子目录的名字随意。我在这里将其命名为tests：
```
$ mkdir tests
```
在tests目录里，创建一个名为ReadingListControllerTest.groovy的新Groovy脚本，编写针对`ReadingListController`的测试。代码清单5-3是个简单的测试，测试控制器能否正确处理HTTP `GET`请求。

__代码清单5-3 `ReadingListController`的Groovy测试__

> 原书P103代码

>**文字**

>Mock ReadingListRepository：模拟`ReadingListRepository`

>Perform and test GET request：执行并测试`GET`请求

如你所见，这就是个简单的JUnit测试，使用了Spring的模拟MVC测试支持功能，对控制器发起`GET`请求。最先设置的是`ReadingListRepository`的一个模拟实现，它会返回一个包含单一`Book`项的列表。随后，测试创建了一个`ReadingListController`实例，将模拟仓库注入`readingListRepository`属性。最后，配置了一个`MockMvc`对象，发起`GET`请求，对期望的视图名称和模型内容进行断言。

但是，此处运行测试要比说明测试更重要。使用CLI的`test`命令，可以像下面这样在命令行里执行测试：
```
$ spring test tests/ReadingListControllerTest.groovy
```
本例中，我明确选中了`ReadingListControllerTest`作为要运行的测试。如果tests/目录里有多个测试，你想要全部运行，可以在`test`命令中指定目录名：
```
$ spring test tests
```
如果你倾向于编写Spock说明而非JUnit测试，那么你一定会很高兴，因为CLI的`test`命令也可以运行Spock说明，代码清单5-4的`ReadingListControllerSpec`就演示了这一功能。

__代码清单5-4 测试`ReadingListController`的Spock说明__

>原文P104代码

>**文字**

>Mock ReadingListRepository：模拟的`ReadingListRepository`

>Perform GET request：执行`GET`请求

>Test results：测试结果

`ReadingListControllerSpec`只是简单地把`ReadingListControllerTest`从JUnit测试翻译成了Spock说明。如你所见，它只是直白地表述了这么一个过程。对“/”出现`GET`请求时，响应中应该包含名为readingList的视图。模型里的`books`键所对应的就是期待的图书列表。

Spock说明也可以通过`spring test tests`来运行`ReadingListControllerSpec`。运行方式和基于JUnit的测试如出一辙。

一旦写好代码，通过了全部测试，你就该部署项目了。让我们来看看Spring Boot CLI是如何帮助产生一个可部署的产物的。

## 5.4 创建可部署的产物

在基于Maven和Gradle的传统Java项目中，构建系统负责产生部署单元——一般是JAR文件或WAR文件。然而，有了Spring Boot CLI，我们可以简单地通过`spring`命令在命令行里运行应用程序。

这是否就意味着要部署一个Spring Boot CLI应用程序，必须在服务器上安装CLI，并手工在命令行里启动应用程序呢？在部署生产环境时，这看起来相当不方便（不用说，这还很危险）。

在第8章里我们会讨论更多部署Spring Boot应用程序的方法。此刻，让我告诉你另一个CLI窍门。针对基于CLI的阅读列表应用程序，在命令行执行如下命令：
```
$ spring jar ReadingList.jar .
```
这会将整个项目打包成一个可执行的JAR文件，包含所有依赖、Groovy和一个嵌入式Tomcat。打包完成后，就可以像下面这样在命令行里运行了（无需CLI）：
```
$ java -jar ReadingList.jar
```
除了可以在命令行里运行外，可执行的JAR文件也能部署到多个平台服务器（Platform as a Service，PaaS）云平台里，包括Pivotal Cloud Foundry和Heroku，在第8章里你会看到相关内容。

## 5.5 小结

Spring Boot CLI利用了Spring Boot自动配置和起步依赖的便利之处，并将其发扬光大。借由Groovy语言的优雅，CLI能让我们在最少的代码噪声下开发Spring应用程序。

本章中我们彻底重写了第2章里的阅读列表应用程序，只是这次我们用Groovy把它写成了Spring Boot CLI应用程序。通过自动添加很多常用包和类的`import`语句，CLI让Groovy更优雅。它还可以自动解析很多依赖库。

对于CLI无法自动解析的库，基于CLI的应用程序可以利用Grape的`@Grab`注解，不用构建说明也能显式地声明依赖。Spring Boot的CLI扩展了`@Grab`注解，针对很多常用库依赖，只需声明Module ID就可以了。

最后，你还了解了如何用Spring Boot CLI来执行测试和构建可部署产物，这些通常都是由构建系统来负责的。

Spring Boot和Groovy结合得很好，两者的简洁性相辅相成。在第6章，我们还会看到Spring Boot和Groovy是如何协同的——Spring Boot是Grails最新版本的核心。
