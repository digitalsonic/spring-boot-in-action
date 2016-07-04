#第6章 在Spring Boot中使用Grails

>本章内容

>* 使用GORM持久化数据
* 定义GSP视图
* Grails 3和Spring Boot入门

我小时候，有一个系列电视广告，当中有两个人，一个在吃巧克力条，另一个在吃罐子里的花生酱。经由一些富有喜剧效果的小事故，两个人撞到了一起。最后，花生酱和巧克力相结合。

一个人说：“你把巧克力弄到我的花生酱里了！”另一个回答：“是你把花生酱弄到我的巧克力上了！”

在一开始的尴尬后，两个人都认同花生酱和巧克力结合在一起是件好事。接着，旁白会建议观众试试Reese牌的的花生酱杯（Peanut Butter Cup）。

在Spring Boot刚发布时，经常有人问我在Spring Boot和Grails之间该如何选择。两者都构建于Spring Framework之上，都旨在简化应用程序的开发。实际上，它们就像花生酱和巧克力。两个都很好，具体如何选择取决于个人爱好。

就像之前巧克力和花生酱的争论一样，事实上并不必从中选出一个来。Spring Boot和Grails两个都很好，完全可以结合到一起。

在本章中，我们会看到Grails和Spring Boot之间的联系。我们会先看到Spring Boot中Grails对象关系映射（Grails Object Relational Mapping，GORM）和Groovy服务器页面（Groovy Server Page，GSP）这样的Grails特性，还会看到Grails 3是如何基于Spring Boot重写的。

## 6.1 使用GORM进行数据持久化

Grails里最让人着迷的恐怕就是GORM了。GORM将数据库相关工作简化到和声明要持久化的实体一样容易。例如，代码清单6-1演示了阅读列表里的`Book`该如何用Groovy写成GORM实体。

__代码清单6-1 GORM `Book`实体__

>原书P108代码

>文字

>This is a GORM entity：这是一个GORM实体

就和`Book`的Java版本一样，这个类里有很多描述图书的属性。但又与Java版本不一样，这里没有分号、`public`或`private`修饰符、setter和getter方法或其他Java中常见的代码噪声。是Grails的`@Entity`注解让这个类变成了GORM实例。这个简单的实体可干了不少事，包括将对象映射到数据库，为`Book`添加持久化方法，通过这些方法可以存取图书。

要在Spring Boot项目里使用GORM，必须在项目里添加GORM依赖。在Maven中，`<dependency>`看起来是这样的：

```
<dependency>
  <groupId>org.grails</groupId>
  <artifactId>gorm-hibernate4-spring-boot</artifactId>
  <version>1.1.0.RELEASE</version>
</dependency>
```

一样的依赖，在Gradle里是这样的：

```
compile("org.grails:gorm-hibernate4-spring-boot:1.1.0.RELEASE")
```

这个库自带了一些Spring Boot自动配置，会自动配置所有支持GORM所需的Bean。你只管写代码就好了。

>
__GORM在Spring Boot里的另一个选择__

> 正如其名，`gorm-hibernate4-spring-boot`是通过Hibernate开启GORM数据持久化的。对很多项目而言，这很好。但如果你想用MongoDB，那你会对Spring Boot里的MongoDB GORM支持很感兴趣。

> 它的Maven依赖是这样的：

>```
<dependency>
  <groupId>org.grails</groupId>
  <artifactId>gorm-mongodb-spring-boot</artifactId>
  <version>1.1.0.RELEASE</version>
</dependency>
```

> 下面是相同的Gradle依赖：
```
compile("org.grails:gorm-mongodb-spring-boot:1.1.0.RELEASE")
```
GORM的工作原理要求实体类必须用Groovy来编写。我们已经在代码清单6-1里写了一个`Book`实体，下面再写一个`Reader`实体：

__代码清单6-2 GORM `Reader`实体__

>原书P109~110

>文字

>This is an entity ：这是一个实体

>Implement UserDetails：实现了`UserDetails`

现在，我们的阅读列表应用程序里有了两个GORM实体，我们需要重写剩下的应用程序来使用这两个实体。因为使用Groovy是如此令人愉悦（和Grails很像），所以其他类我们也会用Groovy来编写。

首先是`ReadingListController`，如代码清单6-3所示。

__代码清单6-3 Groovy的`ReadingListController`__

>原书P110~111

>文字

>Find all reader books：查找读者的全部图书

>Save a book：保存一本书

这个版本的`ReadingListController`和第3章里的相比，最明显的不同之处在于，它是用Groovy写的，没有Java的那些代码噪声。最重要的不同之处在于，无需再注入`ReadingListRepository`，它直接通过`Book`类型持久化。

在`readersBooks()`方法里，它调用了`Book`的`findAllByReader()`静态方法，传入了指定的读者信息。虽然代码清单6-1没有提供`findAllByReader()`方法，但这段代码仍然可以执行，因为GORM会为我们实现这个方法。

与之类似，`addToReadingList()`方法使用了静态方法`withTransaction()`和实例方法`save()`。这两个方法也是GORM提供的，用于将`Book`保存到数据库里。

我们所要做的就是声明一些属性，在`Book`上添加`@Entity`注解。如果你问我怎么看——我觉得这笔买卖很划算。

`SecurityConfig`也要做类似的修改，通过GORM而非`ReadingListRepository`来获取`Reader`。代码清单6-4就是新的`SecurityConfig`。

__代码清单6-4 Groovy版本的`SecurityConfig`__

>原书P111~112代码

>文字

>Find a reader by username：根据用户名查找读者

除了用Groovy重写，`SecurityConfig`里最明显的变化无疑就是第二个`configure()`方法。如你所见，它使用了一个闭包（`UserDetailsService`的实现类），其中调用静态方法`findByUsername()`来查找`Reader`，这个功能是GORM提供的。

你也许会好奇——在这个GORM版本的应用程序里，`ReadingListRepository`变成什么了？GORM替我们处理了所有的持久化工作，这里已经不再需要`ReadingListRepository`了，它的实现也都不需要了。我想你会同意代码越少越好这个观点。

应用程序中剩余的代码也应该用Groovy重写，这样才能和我们的变更相匹配。但它们和GORM没什么关系，也不在本章的讨论范围内。如果想要完整的代码，可以到示范代码页面里去下载。

此刻，你可以通过各种运行Spring Boot应用程序的方法来启动阅读列表应用程序。启动后，应用程序应该能像从前一样工作。只有你我知道持久化机制已经被改变了。

除了GORM，Grails应用程序通常还会用Groovy Server Pages将模型数据以HTML的方式呈现给浏览器。6.2节应用程序的Grails化还会继续。我们会把Thymeleaf替换为等价的GSP。

## 6.2 使用Groovy Server Pages定义视图

到目前为止，我们都在用Thymeleaf模板定义阅读列表应用程序的视图。除了Thymeleaf，Spring Boot还支持Freemarker、Velocity和基于Groovy的模板。无论选择哪种模板，你要做的就是添加合适的起步依赖，在Classpath根部的templates/目录里编写模板。自动配置会处理剩下的事情。

Grails项目也提供GSP的自动配置。如果你想在Spring Boot应用程序里使用GSP，必须向项目里添加Spring Boot的GSP库：
```
compile("org.grails:grails-gsp-spring-boot:1.0.0")
```
和Spring Boot提供的其他视图模板一样，库放在Classpath里就会触发自动配置，设置所需的视图解析器，以便在Spring MVC的视图层里使用GSP。

剩下的就是为应用程序编写GSP模板了。在阅读列表应用程序中，我们要把Thymeleaf的readingList.html文件用GSP的形式重写，放在readingList.gsp文件（位于src/main/resources/templates）里。代码清单6-5就是新的GSP模板的代码。

__代码清单6-5 GSP编写的阅读列表应用程序主视图__

>原书P113~114代码

>文字

>List the books：罗列图书

>The book form：图书表单

>The CSRF token： CSRF令牌

如你所见，GSP模板中使用了表达式语言引用（用`${}`包围的部分）以及GSP标签库（例如`<g:if>`和`<g:each>`）。这并不是Thymeleaf那样的纯HTML。但如果习惯用JSP，你会很熟悉这种方式，而且会觉得这是一个不错的选择。

代码里的绝大部分内容和第2章、第3章的Thymeleaf模板类似，映射GSP模板上的元素。但是有一点要注意，你必须要放一个隐藏域，其中包含CSRF（Cross-Site Request Forgery）令牌。Spring Security在提交`POST`请求时要求带有这个令牌，Thymeleaf在呈现HTML时会自动包含这个令牌，但在GSP里你必须在隐藏域显式地包含它。

图6-1是GSP模板的显示效果，其中添加了一些图书。

虽然GORM和GSP这样的Grails特性很吸引人，让Spring Boot应用程序更简单，但你在这里还不能真正体验Grails。让我们再往Spring Boot的花生酱里放一点Grails巧克力。现在让我们来看看Grails 3如何将两者合二为一，带来完整的Spring Boot和Grails开发体验。

>P115图

__图6-1 使用了GSP模板的阅读列表__

## 6.3 结合Spring Boot与Grails 3

Grails一直都是构建于Spring、Groovy、Hibernate和其他巨人肩膀之上的高阶框架。到了Grails 3，Grails已经基于Spring Boot，带来了令人愉悦的开发体验。Grails开发者和Spring Boot开发者都能驾轻就熟。

要使用Grails 3，首先要进行安装。在Mac OS X和大部分Unix系统上，最简单的安装方法是在命令行里使用SDKMAN：
```
$ sdk install grails
```
如果你用的是Windows，或者无法使用SDKMAN，就需要下载二进制发布包。解压后要将bin目录添加到系统路径里去。

无论用哪种安装方式，你都可以在命令行中查看Grails的版本，验证安装是否成功：
```
$ grails -version
```
如果安装成功，现在就可以创建Grails项目了。

### 6.3.1 创建新的Grails项目

在Grails项目中，你会使用`grails`命令行工具执行很多任务，包括创建项目。要创建阅读列表项目，可以这样使用`grails`命令：
```
$ grails create-app readinglist
```
正如这个命令的名字所示，`create-app`命令创建了一个新的应用程序项目。在这个例子里，项目名是readinglist。

等`grails`工具创建完应用程序，`cd`到了`readinglist`目录里，看看所创建的内容吧。图6-2应该就是你看到的项目结构的概览。

在这个项目目录结构里，你应该认出了一些熟悉的东西。这里有一个Gradle的构建说明文件和配置（build.gradle和gradle.properties）。src目录里还有一个标准的Gradle项目结构，但是grails-app应该是里面最有趣的目录。如果用过老版本的Grails，你就会知道这个目录的作用。这里面放的是你写的控制器、领域类和其他构成Grails项目的代码。

>P116[图

__图6-2 Grails 3项目的目录结构__

如果再深挖一下，打开build.gradle文件，会发现一些更熟悉的东西。首先，构建说明文件里使用了Spring Boot的Gradle插件：
```
apply plugin: "spring-boot"
```
这意味着你能像使用其他Spring Boot应用程序那样构建并运行这个Grails应用程序。

你还应该注意到，依赖里有不少有用的Spring Boot库：

```
dependencies {
  compile 'org.springframework.boot:spring-boot-starter-logging'
  compile("org.springframework.boot:spring-boot-starter-actuator")
  compile "org.springframework.boot:spring-boot-autoconfigure"
  compile "org.springframework.boot:spring-boot-starter-tomcat"
  ...
}
```
这些库为Grails应用程序提供了Spring Boot的自动配置、日志，还有Actuator及嵌入式Tomcat。把应用当作可执行JAR运行时，这个Tomcat可以提供服务。

实际上，这是一个Spring Boot项目，同时也是Grails项目，因为Grails 3就是构建在Spring Boot的基础上的。

#### 运行应用程序

运行Grails应用程序最直接的方式是在命令行里使用`grails`工具的`run-app`命令：
```
$ grails run-app
```
就算一行代码都还没写，我们也能运行应用程序，在浏览器里进行访问。一旦应用程序启动，就可以在浏览器里访问[http://localhost:8080](http://localhost:8080)。你应该能看到类似图6-3的页面。

>P117图

__图6-3 全新的Grails应用程序__

在Grails里运行应用程序要使用`run-app`命令，这种方式已经用了很多年，上个版本的Grails也是这样。Grails 3项目的Gradle说明里使用了Spring Boot的Gradle插件，你还可以用各种运行Spring Boot项目的方式来运行这个应用程序。此处通过Gradle引入了`bootRun`任务：
```
$ gradle bootRun
```
你还可以构建项目，运行生成的可执行JAR文件：
```
$ gradle build
...
$ java -jar build/lib/readingList-0.1.jar
```
当然，构建产生的WAR文件可以部署到你喜欢的各种Servlet 3.0容器里。

在开发早期就能运行应用程序，这一点十分方便，能帮你确认应用程序已正确初始化。但是这时应用程序还没做什么有意思的事情，在初始化后的项目上做什么完全取决于我们。接下来，开始定义领域模型吧。

### 6.3.2 定义领域模型

阅读列表应用程序里的核心领域模型是`Book`类。虽然我们可以手工创建Book.groovy文件，但通常还是用`grails`工具来创建领域模型类比较好。因为它知道该把文件放到哪里，并且能在同一时间生成各种相关内容。

要创建`Book`类，我们会使用`grails`工具的`create-domain-class`命令：
```
$ grails create-domain-class Book
```
这条命令会生成两个源文件：一个Book.groovy文件和一个BookSpec.groovy文件。后者是一个Spock说明，用来测试`Book`类。一开始这个文件是空的，你可以填入各种测试内容来验证`Book`的各种功能。

Book.groovy文件里定义了`Book`类，你可以在grails-app/domain/readingList里找到这个文件。它一开始基本没什么内容：
```
package readinglist
class Book {

  static constraints = {
  }
}
```
我们需要添加一些字段来定义一本书，比如书名、作者和ISBN。在添加了这些字段后，Book.groovy看起来是这样的：
```
package readinglist
class Book {
  static constraints = {
  }

  String reader
  String isbn
  String title
  String author
  String description

}
```
静态的`constraints`变量里可以定义各种附加在`Book`实例上的验证约束。本章中，我们主要关注阅读列表应用程序的构建，看看如何基于Spring Boot构建应用程序，不会太关注验证的问题。因此，这里的`constraints`内容为空。当然，如果有需要的话，你可以随意添加约束。可以参考一下*Grails in Action  Second Edition *（Manning，2014），作者是Glen Smith和Peter Ledbrook。***{![虽然*Grails in Action Second Edition*主讲Grails 2，但你在Grails 2里了解到的大部分内容都适用于Grails 3。]}***

为了使用Grails，我们要保持阅读列表应用程序的简洁，要和第2章的程序一致。因此，接下来我们要创建`Reader`领域模型，还有控制器。

### 6.3.3 开发Grails控制器

有了领域模型，通过`grails`工具创建出控制器就很容易了。关于控制器，有几个命令可供选择。

* `create-controller`：创建空控制器，让开发者来编写控制器的功能。
* `generate-controller`：生成一个控制器，其中包含特定领域模型类的基本CRUD操作。
* `generate-all`：生成针对特定领域模型类的基本CRUD控制器，及其视图。

脚手架控制器很好用，也是Grails中比较知名的特性，但我们仍然会保持简洁，写一个仅包含必要功能的控制器，能匹配第2章里的应用程序功能就好。因此，我们用`create-controller`命令来创建原始的控制器，然后填入所需的方法：
```
$ grails create-controller ReadingList
```
这个命令会在grails-app/controllers/readingList里创建一个名为`ReadingListController`的控制器：
```
package readinglist
class ReadingListController {

  def index() { }
}
```
一行代码都不用改，这个控制器就能运行了，虽然它干不成什么事。此时，它能处理发往/readingList的请求，将请求转给grails-app/views/readingList/index.gsp里定义的视图（现在还没有，我们稍后会创建的）。

我们需要控制器来显示图书列表，还有添加新书的表单。我们还需要提交表单，将新书保存到数据库里的功能。下面的代码就是我们所需要的`ReadingListController`。

__代码清单6-6 改写`ReadingListController`__

>原书120代码

>文字

>Fetch books into model：获取图书填充到模型里

>Save the book：保存图书

虽然相比等效的Java控制器，代码长度大大缩短，但这个版本的`ReadingListController`功能已经基本完整。它可以处理发往/readingList的`GET`请求，获取并展示图书列表。在表单提交后，它还会处理`POST`请求，保存图书，随后重定向回index动作（由`index()`方法来处理）。

太不可思议了，我们已经基本完成了Grails版本的阅读列表应用程序。剩下的就是创建一个视图，显示图书列表和表单。

### 6.3.4 创建视图

Grails应用程序通常都用GSP模板来做视图。你已经看到过如何在Spring Boot应用程序里使用GSP了，因此，此处的模板并不会和6.2节里的模板有太多不同。

我们要做的是，利用Grails提供的布局设施，将公共的设计风格运用到整个应用程序里。如代码清单6-7所示，这就是个很简单的修改。

__代码清单6.7 一个适用于Grails的GSP模板，包含布局__

```
<!DOCTYPE html>
<html>
  <head>
    <meta name="layout" content="main"/>
    <title>Reading List</title>
    <link rel="stylesheet"
          href="/assets/main.css?compile=false"  />
    <link rel="stylesheet"
          href="/assets/mobile.css?compile=false"  />
    <link rel="stylesheet"
          href="/assets/application.css?compile=false"  />
  </head>

  <body>
    <h2>Your Reading List</h2>

    <g:if test="${bookList && !bookList.isEmpty()}">
      <g:each in="${bookList}" var="book">
      <dl>
        <dt class="bookHeadline">
          ${book.title}</span> by ${book.author}
          (ISBN: ${book.isbn}")
        </dt>
        <dd class="bookDescription">
          <g:if test="${book.description}">
          ${book.description}
          </g:if>
          <g:else>
          No description available
          </g:else>
        </dd>
      </dl>
      </g:each>
    </g:if>
    <g:else>
      <p>You have no books in your book list</p>
    </g:else>

    <hr/>

    <h3>Add a book</h3>
    <g:form action="save">
    <fieldset class="form">
      <label for="title">Title:</label>
      <g:field type="text" name="title" value="${book?.title}"/><br/>
      <label for="author">Author:</label>
      <g:field type="text" name="author"
                          value="${book?.author}"/><br/>
      <label for="isbn">ISBN:</label>
      <g:field type="text" name="isbn" value="${book?.isbn}"/><br/>
      <label for="description">Description:</label><br/>
      <g:textArea name="description" value="${book?.description}"
                                     rows="5" cols="80"/>
    </fieldset>
    <fieldset class="buttons">
      <g:submitButton name="create" class="save"
        value="${message(code: 'default.button.create.label',
                                              default: 'Create')}" />
    </fieldset>
    </g:form>

  </body>
</html>
```

>文字

>Use the main layout：使用了`main`布局

>List the books：列出图书

>The book form：图书表单

在`<head>`元素里我们移除了引用样式表的`<link>`标签。这里放了一个`<meta>`标签，引入了Grails应用程序的`main`布局。这样一来，应用程序就能用上Grails的外观了，运行的效果如图6-4所示。

>P122图

__图6-4 应用通用Grails风格的阅读列表应用程序__

虽然Grails风格比之前用的简单的样式表更吸引眼球。但很显然的是，要让阅读列表应用程序更好看，还有一些工作要做。首先要让应用程序和Grails不那么像，和我们的想象更接近一点。修改应用程序的样式表在本书的讨论范围之外，但如果你对样式微调感兴趣，可以在grails-app/assets/stylesheets目录里找到样式表文件。

## 6.4 小结

Grails和Spring Boot都旨在让开发者的生活更简单，大大简化基于Spring的开发模型，因此两者看起来是互相竞争的框架。但在本章中，我们看到了两者如何结合在一起，综合优势。

我们了解了如何向典型的Spring Boot应用程序中添加GORM和GSP视图，这两个都是知名的Grails特性。GORM是Spring Boot里一个很受欢迎的特性，能让你直接针对领域模型执行持久化操作，消除了对模型仓库的需求。

随后我们认识了Grails 3，即Grails构建于Spring Boot之上的最新版本。在开发Grails 3应用程序时，你也在使用Spring Boot，可以使用Spring Boot的全部特性，包括自动配置。

在本章和第5章里，我们看到了如何结合Groovy和Spring Boot，消除Java语言无法避免的那些代码噪声。

在第7章，我们要将关注点从开发Spring Boot应用程序转移到Spring Boot Actuator上，看看它如何帮助我们了解应用程序的运行情况。
