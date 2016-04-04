# Getting Groovy with the Spring Boot CLI
# Groovy与Spring Boot CLI

__This chapter covers__  
__本章内容涉及__

* Automatic dependencies and imports
* Grabbing dependencies
* Testing CLI-based applications
* 自动依赖与import
* 获取依赖
* 测试基于CLI的应用程序

Some things go really well together. Peanut butter and jelly. Abbott and Costello. Thunder and lightning. Milk and cookies. On their own, these things are great. But when paired up, they’re even more awesome.  
有些东西真的很适合放在一起，花生酱和果酱，Abbott和Costello[译注1][Y1]，打雷和闪电，牛奶和饼干。每样东西都很棒，但如果能搭配起来那就更赞了。

So far, we’ve seen a lot of great things that Spring Boot has to offer, including auto-configuration and starter dependencies. When paired up with the elegance of
the Groovy language, the result can be greater than the sum of its parts.  
到目前为止，我们已经看到了Spring Boot带来的不少好东西，包括自动配置和起步依赖。要是再能搭配上Groovy的优雅，就能起到一加一大于二的效果。

In this chapter, we’re going to look at the Spring Boot CLI, a command-line tool that brings the power of Spring Boot and Groovy together to form a simple and
compelling development tool for creating Spring applications. To demonstrate the power of Spring Boot’s CLI, we’re going to rewind the reading-list application from chapter 2, rewriting it from scratch in Groovy and taking advantage of the benefits that the CLI has to offer.  
在本章中，我们会了解到Spring Boot CLI，这是一个命令行工具，将强大的Spring Boot和Groovy结合到一起，形成了一套针对Spring应用程序的既简单又引人入胜的开发工具。要演示Spring Boot CLI的强大，我们会回到第2章的阅读列表应用程序，用Groovy重新这个应用程序，利用好CLI带来的好处。

[Y1]: # "两位都是美国的喜剧演员，详见[https://en.wikipedia.org/wiki/Abbott_and_Costello](https://en.wikipedia.org/wiki/Abbott_and_Costello)。"

## 5.1 Developing a Spring Boot CLI application
## 5.1 开发Spring Boot CLI应用程序

Most development projects that target the JVM platform are developed in the Java language and involve a build system such as Maven or Gradle to produce a deployable artifact. In fact, the reading-list application we created in chapter 2 follows this model.  
大部分针对JVM平台的项目都是用Java语言来开发的，引入了诸如Maven或Gradle这样构建系统来生成可部署的产物。实际上，我们在第2章里开发的阅读列表应用程序就是遵循这套模型的。

The Java language has seen great improvements in recent versions. Even so, Java has a few strict rules that add noise to the code. Line-ending semicolons, class and method modifiers (such as public and private), getter and setter methods, and import statements serve a purpose in Java, but they distract from the essentials of the code. From a developer’s perspective, code noise is friction—friction when writing code and even more friction when trying to read it. If some of this code noise could be eliminated, it’d be easier to develop and read the code.  
在最近的版本中Java语言有不少改进，但就算如此，Java还是有一些严格的规则为代码增加了不少噪声。行尾分号、类和方法的修饰符（比如`public`和`private`）、getter和setter方法，还有`import`语句在Java中都有自己的作用，但它们干扰了代码的本质。从开发者的角度来看，代码噪声是冲突——编写代码时的冲突，在试图阅读代码时更是冲突。如果能消除一部分代码噪声，就能更方便地开发和阅读代码。

Likewise, build systems such as Maven and Gradle serve a purpose in a project. But build specifications are also one more thing that must be developed and maintained. If only there were a way to do away with the build, projects would be that much simpler.  
同样的，Maven和Gradle这样的构建系统在项目中也有自己的作用，但你还得开发和维护构建说明。如果能直接构建，项目也会更简单一些。

When working with the Spring Boot CLI, there is no build specification. The code itself serves as the build specification, providing hints that guide the CLI in resolving dependencies and producing deployment artifacts. Moreover, by teaming up with Groovy, the Spring Boot CLI offers a development model that eliminates almost all code noise, producing a friction-free development experience.  
在使用Spring Boot CLI时，没有构建说明文件。代码本身就是构建说明，提供了一些线索指引CLI解析依赖并生成用于部署的产物。此外，配合上Groovy，Spring Boot CLI提供了一种开发模型，消除了几乎所有代码噪声，带来了毫无违和感的开发体验。

In the very simplest case, writing a CLI-based application could be as easy as writing a single standalone Groovy script like the one we wrote in chapter 1. But when writing a more complete application with the CLI, it makes sense to set up a basic project structure to house the project code. That’s where we’ll get started with rewriting the reading-list application.  
在最简单的情况下，编写基于CLI的应用程序就和编写单独的Groovy脚本一样简单，就像第1章里写的那个脚本。但要用CLI编写更完整的应用程序，就需要设置一个基本的项目结构来容纳项目代码。我们马上就要用它来重写阅读列表应用程序了。

### 5.1.1 Setting up the CLI project
### 5.1.1 设置CLI项目

The first thing we’ll do is create a directory structure to house the project. Unlike Maven- and Gradle-based projects, Spring Boot CLI projects don’t have a strict project structure. In fact, the simplest Spring Boot CLI app could be a single Groovy script living in any directory in the filesystem. For the reading-list project, however, you should create a fresh, clean directory to keep the code separate from anything else you keep on your machine:  
我们要做的第一件事是创建目录结构来容纳我们的项目。与基于Maven或Gradle的项目不同，Spring Boot CLI项目并没有严格的项目结构要求。实际上，最简单的Spring Boot CLI应用程序就是一个Groovy脚本，可以放在文件系统的任意目录里。对阅读列表应用程序而言，你应该创建一个新的、干净的目录来存放代码，把它们和你电脑上的其他东西分开：

```
$ mkdir readinglist
```

Here I’ve named the directory “readinglist”, but feel free to name it however you wish. The name isn’t as important as the fact that the project will have a place to live.  
此处我将目录命名为“readinglist”，但你可以随意命名。比起找个地方放置代码，名字并不重要。

We’ll need a couple of extra directories to hold the static web content and the Thymeleaf template. Within the readinglist directory, create two new directories named “static” and “templates”:  
我们还需要两个额外的目录存放静态Web内容和Thymeleaf模板。在readinglist目录里，创建两个新的目录名为“static”和“templates”：

```
$ cd readinglist
$ mkdir static
$ mkdir templates
```

It’s no coincidence that these directories are named the same as the directories we created under src/main/resources in the Java-based project. Although Spring Boot doesn’t enforce a project structure like Maven and Gradle do, Spring Boot will still auto-configure a Spring ResourceHttpRequestHandler that looks for static content in a directory named “static” (among other locations). It will also still configure Thymeleaf to resolve templates from a directory named “templates”.  
这些目录的名字和基于Java的项目中src/main/resources里的目录同名。虽然Spring Boot并不强制要求和Maven和Gradle一样的目录结构，但Spring Boot会自动配置一个Spring `ResourceHttpRequestHandler`查找“static”目录里的静态内容（还有其他位置）。还会配置Thymeleaf来解析“templates”目录里的模板。

Speaking of static content and Thymeleaf templates, those files will be exactly the same as the ones we created in chapter 2. So that you don’t have to worry about remembering them later, go ahead and copy style.css into the static directory and readingList.html into templates.  
说道静态内容和Thymeleaf模板，那些文件的内容和我们在第2章里创建的一样。因此你不用担心稍后要回忆它们，直接把style.css复制到static目录，把readingList.html复制到templates目录。

At this point the reading-list project should be structured like this:  
此时，阅读列表项目的目录结构应该是这样的：

```
.
|---static
|   |---style.css
|---templates
    |---readingList.html
```

Now that the project is set up, we’re ready to start writing some Groovy code.  
现在项目已经设置好了，我们准备好编写Groovy代码了。

### 5.1.2 Eliminating code noise with Groovy
### 5.1.2 通过Groovy消除代码噪声

On its own, Groovy is a very elegant language. Unlike Java, Groovy doesn’t require qualifiers such as public and private. Nor does it demand semicolons at the end of each line. Moreover, thanks to Groovy’s simplified property syntax (“GroovyBeans”), the JavaBean standard accessor methods are unnecessary.  
就语言本身而言，Groovy是种优雅的语言。与Java不同，Groovy并不要求`public`和`private`这样的限定符，也不要求在行尾有分号。此外，归功于Groovy的简化属性语法（“GroovyBeans”），JavaBean的标准访问方法没有存在的必要了。

Consequently, writing the Book domain class in Groovy is extremely easy. Create a new file at the root of the reading-list project named Book.groovy and write the following Groovy class in it.  
随之而来的结果是用Groovy编写出的Book领域类相当简单。在阅读列表项目的根目录里创建一个新的文件，名为“Book.groovy”，其中编写如下Groovy类。

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

As you can see, this Groovy class is a mere fraction of the size of its Java counterpart. There are no setter or getter methods, no public or private modifiers, and no semicolons. The code noise that is so common in Java is squelched, and all that’s left is what describes the essence of a book.  
如你所见，Groovy类与它的Java参照物相比，在大小上完全不在一个量级，其中没有setter和getter方法，没有`public`和`private`修饰符，没有分号。在Java中常见的代码噪声不复存在，剩下的内容都是在描述书的基本信息。

>__JDBC vs. JPA in the Spring Boot CLI__  
__Spring Boot CLI中的JDBC与JPA__

>One difference you may have noticed between this Groovy implementation of Book and the Java implementation in chapter 2 is that there are no JPA annotations. That’s because this time we’re going to use Spring’s JdbcTemplate to access the database instead of Spring Data JPA.  
你也许已经注意到了，`Book`的Groovy实现与第2章里的Java实现有所不同，上面没有添加JPA注解。因为在这里我们要用Spring的`JdbcTemplate`来访问数据库，而非Spring Data JPA。

>There are a couple of very good reasons why I chose JDBC instead of JPA for this example. First, by mixing things up a little, I can show off a few more auto-configuration tricks that Spring Boot performs when working with Spring’s JdbcTemplate. But perhaps the most important reason I chose JDBC is that Spring Data JPA requires a .class file when generating on-the-fly implementations of repository interfaces. When you run Groovy scripts via the command line, the CLI compiles the scripts in memory and doesn’t produce .class files. Therefore, Spring Data JPA doesn’t work well when running scripts through the CLI.  
有好几个不错的理由能解释为什么我在这个例子里选择JDBC而非JPA。首先，在使用Spring的`JdbcTemplate`时，我可以多用几种不同的方法，展示Spring Boot的更多自动配置技巧。但我选择JDBC的最主要原因是Spring Data JPA在生成仓库接口的自动实现时要求有一个.class文件。当你在命令行里运行Groovy脚本时，CLI会在内存里编译脚本，并不会产生.class文件。因此，当你在CLI里运行脚本时Spring Data JPA并不适用。

>That said, the CLI isn’t completely incompatible with Spring Data JPA. If you use the CLI’s jar command to package your application into a JAR file, the resulting JAR file will contain compiled .class files for all of your Groovy scripts. Building and running a JAR file from the CLI is a handy option when you want to deploy an application developed with the CLI, but it isn’t as convenient during development when you want to see the results of your work quickly.  
但也不是说CLI和Spring Data JPA完全不兼容。如果使用CLI的`jar`命令把应用程序打包成一个JAR文件，结果文件里会包含所有Groovy脚本编译后的.class文件。当你想部署一个用CLI开发的应用程序时，在CLI里构建并运行JAR文件是一个不错的选择。但是如果你想在开发时快速看到开发内容的效果，这种做法就没那么方便了。

Now that we’ve defined the Book domain class, let’s write the repository. First, let’s write the ReadingListRepository interface (in ReadingListRepository.groovy):  
既然我们定义好了`Book`领域类，就开始编写仓库接口吧。首先，编写`ReadingListRepository`接口（位于ReadingListRepository.groovy）：

```
interface ReadingListRepository {

    List<Book> findByReader(String reader)

    void save(Book book)

}
```

Aside from a clear lack of semicolons and no public modifier on the interface, the Groovy version of ReadingListRepository isn’t much different from its Java counterpart. The most significant difference is that it doesn’t extend JpaRepository because we’re not working with Spring Data JPA in this chapter. And since we’re not using Spring Data JPA, we’re going to have to write the implementation of ReadingListRepository ourselves. The following listing shows what JdbcReadingListRepository.groovy should look like.  
除了没有分号，以及接口上没有`public`修饰符，`ReadingListRepository`的Groovy版本和与之对应的Java版本并无二致。最显著的区别是它没有扩展`JpaRepository`，因为本章中我们并不适用Spring Data JPA。既然不用Spring Data JPA，我们就不得不自己来实现`ReadingListRepository`了。下面的代码就是JdbcReadingListRepository.groovy的内容：

__Listing 5.1 A Groovy and JDBC implementation of ReadingListRepository__  
__代码5.1 ReadingListRepository的Groovy JDBC实现__

```
@Repository
class JdbcReadingListRepository implements ReadingListRepository {

  @Autowired
  JdbcTemplate jdbc

  List<Book> findByReader(String reader) {
    jdbc.query(
        "select id, reader, isbn, title, author, description " +
        "from Book where reader=?",
        { rs, row ->
              new Book(id: rs.getLong(1),
                  reader: rs.getString(2),
                  isbn: rs.getString(3),
                  title: rs.getString(4),
                  author: rs.getString(5),
                  description: rs.getString(6))
        } as RowMapper,
        reader)
  }

  void save(Book book) {
    jdbc.update("insert into Book " +
                "(reader, isbn, title, author, description) " +
                "values (?, ?, ?, ?, ?)",
        book.reader,
        book.isbn,
        book.title,
        book.author,
        book.description)
  }

}
```

Inject JdbcTemplate  
注入`JdbcTemplate`

RowMapper closure  
`RowMapper`闭包

For the most part, this is a typical JdbcTemplate-based repository implementation. It’s injected, via autowiring, with a reference to a JdbcTemplate object that it uses to query the database for books (in the findByReader() method) and to save books to the database (in the save() method).  
以上代码的大部分内容是一个典型的基于`JdbcTemplate`的仓库实现。它通过自动织入注入了一个`JdbcTemplate`对象的引用，用它查询数据库获取图书（在`findByReader()`方法里），将图书保存到数据库（在`save()`方法里）。

By writing it in Groovy, we’re able to apply some Groovy idioms in the implementation. For example, in findByReader(), a Groovy closure is given as a parameter in the call to query() in place of a RowMapper implementation.1 Also, within the closure, a new Book object is created using Groovy’s support for setting object properties at construction.  
由于是用Groovy编写的，我们在实现中可以使用一些Groovy的语法糖。举个例子，在`findByReader()`里，调用`query()`时在需要`RowMapper`实现的地方传入了一个Groovy闭包。<sup>[1][]</sup>此外，在闭包中创建了一个新的`Book`对象，在构造时设置对象的属性。

While we’re thinking about database persistence, we also need to create a file named schema.sql that will contain the SQL necessary to create the Book table that the repository issues queries against:  
在考虑数据库持久化时，我们还需要创建一个名为schema.sql的文件，其中包含了创建`Book`表所需的SQL：

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

[1]: # "In fairness to Java, we can do something similar in Java 8 using lambdas (and method references). 为了公平对待Java，在Java 8里我们可以用Lambda（和方法引用）做类似的事情。"

I’ll explain how schema.sql is used later. For now, just know that you need to create it at the root of the classpath (at the root directory of the project) so that there will actually be a Book table to query against.  
稍后我会解释如何使用schema.sql，现在你只需要知道把它放在Classpath的根目录（即项目的根目录），就会创建出查询用到的`Book`表了。

All of the Groovy pieces are coming together, but there’s one more Groovy class we must write to make the Groovy-ified reading-list application complete. We need to write a Groovy implementation of ReadingListController to handle web requests and serve the reading list to the browser. At the root of the project, create a file named ReadingListController.groovy with the following content.  
所有的Groovy部分差不多都到齐了，但还有一个必须要写的Groovy类，这样Groovy化的阅读列表应用程序才能完整。我们需要编写一个`ReadingListController`的Groovy实现来处理Web请求，为浏览器提供阅读列表。在项目的根目录，创建一个名为ReadingListController.groovy文件，内容如下。

__Listing 5.2 ReadingListController handles web requests for displaying and adding__
__代码5.2 处理展示和添加Web请求的`ReadingListController`__

```
@Controller
@RequestMapping("/")
class ReadingListController {

  String reader = "Craig"

  @Autowired
  ReadingListRepository readingListRepository

  @RequestMapping(method=RequestMethod.GET)
  def readersBooks(Model model) {
    List<Book> readingList =
        readingListRepository.findByReader(reader)

    if (readingList) {
      model.addAttribute("books", readingList)
    }

    "readingList"
  }

  @RequestMapping(method=RequestMethod.POST)
  def addToReadingList(Book book) {
    book.setReader(reader)
    readingListRepository.save(book)
    "redirect:/"
  }

}
```

Inject ReadingListRepository  
注入`ReadingListRepository`

Fetch reading list  
获取阅读列表

Populate model  
设置模型

Return view name  
返回视图名称

Save book  
保存图书

Redirect after POST  
POST后重定向

Comparing this version of ReadingListController with the one from chapter 2, it’s easy to see that there’s a lot in common. The main difference, once again, is that Groovy’s syntax does away with class and method modifiers, semicolons, accessor methods, and other unnecessary code noise.  
和第2章里的版本相比，这个`ReadingListController`和它有很多相似之处。主要的不同在于Groovy的语法消除了类和方法的修饰符、分号、访问方法和其他不必要的代码噪声。

You’ll also notice that both handler methods are declared with def rather than String and both dispense with an explicit return statement. If you prefer explicit typing on the methods and explicit return statements, feel free to include them— Groovy won’t mind.  
你还会注意到两个处理器方法都是用`def`来定义的，而非`String`；两者都没有显式的`return`语句。如果你喜欢在方法上说明类型，有显式的`retrun`语句，加上就好了——Groovy并不在意这些细节。

There’s one more thing we need to do before we can run the application. Create a
new file named Grabs.groovy and put these three lines in it:  
在运行应用程序之前，还要做一件事，创建一个新文件，名为Grabs.groovy，内容是如下三行：

```
@Grab("h2")
@Grab("spring-boot-starter-thymeleaf")
class Grabs {}
```

We’ll talk more about what this class does later. For now, just know that the @Grab annotations on this class tell Groovy to fetch a few dependency libraries on the fly as the application is started.  
稍后我们再来讨论这个类的作用，现在你只需要知道类上的`@Grab`注解会告诉Groovy在启动应用程序时自动去获取一些依赖的库。

Believe it or not, we’re ready to run the application. We’ve created a project directory, copied a stylesheet and Thymeleaf template into it, and filled it with Groovy code. All that’s left is to run it with the Spring Boot CLI (from within the project directory):  
不管你信还是不信，我们已经可以运行这个应用程序了。我们创建了一个项目目录，向其中复制了一个样式表和Thymeleaf模板，填充了一些Groovy代码。剩下的就是用Spring Boot CLI（在项目目录里）来运行它了：

```
$ spring run .
```

After a few seconds, the application should be fully started. Open your web browser and navigate to http://localhost:8080. Assuming everything goes well, you should see the same reading-list application you saw in chapter 2.  
几秒后，应用程序就完全启动起来了。打开浏览器，访问http://localhost:8080。如果一切正常，你应该就能看到和第2章里一样的阅读列表应用程序了。

Success! In just a few pages of this book, you’ve written a complete (albeit simple) Spring application!  
成功啦！只用了几页纸的篇幅，你就写了一个完整（相对简单）的Spring应用程序！

At this point, however, you might be wondering how it works, considering that...  
然而，此时此刻你也许会好奇，这是怎么办到的……

* There’s no Spring configuration. How are the beans created and wired together? Where does the JdbcTemplate bean come from?
* There’s no build file. Where do the library dependencies like Spring MVC and Thymeleaf come from?
* There are no import statements. How can Groovy resolve types like JdbcTemplate and RequestMapping if there are no import statements to specify what packages they’re in?
* We never deployed the app. Where’d the web server come from?
* ___没有Spring配置。___Bean是如何创建并组装的？`JdbcTemplate` Bean又是从哪来的？
* ___没有构建文件。___Spring MVC和Thymeleaf这样的依赖库是哪来的？
* ___没有`import`语句。___如果没有`import`语句来指定具体的包，Groovy是如何解析`JdbcTemplate`和`RequestMapping`类型的？

Indeed, the code we’ve written seems to be missing more than just a few semicolons. How does this code even work?  
实际上，我们编写的代码看起来缺少的不止是分号。这些代码究竟是怎么跑起来的？

### 5.1.3 What just happened?
### 5.1.3 发生了什么？

As you’ve probably surmised, there’s more to Spring Boot’s CLI than just a convenient means of writing Spring applications with Groovy. The Spring Boot CLI has several tricks in its repertoire, including the following:  
如你所猜测的那样，此处不仅是用Groovy编写Spring应用程序，更多的是Spring Boot的CLI。Spring Boot CLI使劲了浑身解数，包括：

* The CLI is able to leverage Spring Boot auto-configuration and starter dependencies.
* The CLI is able to detect when certain types are in use and automatically resolve the appropriate dependency libraries to support those types.
* The CLI knows which packages several commonly used types are in and, if those types are used, adds those packages to Groovy’s default packages.
* By applying both automatic dependency resolution and auto-configuration, the CLI can detect that it’s running a web application and automatically include an embedded web container (Tomcat by default) to serve the application.
* CLI可以利用Spring Boot的自动配置和起步依赖。
* CLI可以检测到正在使用的特定类，自动解析合适的依赖库来支持那些类。
* CLI知道多数常用类都在哪些包里，如果用到了这些类，它会把那些包加入Groovy的默认包里。
* 应用了自动依赖解析和自动配置后，CLI可以检测到当前运行的是一个Web应用程序，自动引入嵌入式Web容器（默认是Tomcat）供应用程序使用。

If you think about it, these are the most important features that the CLI offers. The Groovy syntax is just a bonus!  
如果你仔细想想，这些事CLI提供的最重要的特性。Groovy语法只是额外的福利！

When you run the reading-list application through the Spring Boot CLI, several things happen under the covers to make this magic work. One of the very first things the CLI does is attempt to compile the Groovy code using an embedded Groovy compiler. Without you knowing it, however, the code fails to compile due to several unknown types in the code (such as JdbcTemplate, Controller, RequestMapping, and so on).

But the CLI doesn’t give up. The CLI knows that JdbcTemplate can be added to the classpath by adding the Spring Boot JDBC starter as a dependency. It also knows that the Spring MVC types can be found by adding the Spring Boot web starter as a dependency. So it grabs those dependencies from the Maven repository (Maven Central, by default).

If the CLI were to try to recompile at this point, it would still fail because of the missing import statements. But the CLI also knows the packages of many commonly used types. Taking advantage of the ability to customize the Groovy compiler’s default package imports, the CLI adds all of the necessary packages to the Groovy compiler’s default imports list.

Now it’s time for the CLI to attempt another compile. Assuming there are no other problems outside of the CLI’s abilities (such as syntax errors or types that the CLI doesn’t know about), the code will compile cleanly and the CLI will run it via an internal bootstrap method similar to the main() method we put in Application for the Java- based example.

At this point, Spring Boot auto-configuration kicks in. It sees that Spring MVC is on the classpath (as a result of the CLI resolving the web starter), so it automatically configures the appropriate beans to support Spring MVC, as well as an embedded Tomcat bean to serve the application. It also sees that JdbcTemplate is on the classpath, so it automatically creates a JdbcTemplate bean, wiring it with a DataSource bean that was also automatically created.

Speaking of the DataSource bean, it’s just one of several other beans that are created via Spring Boot auto-configuration. Spring Boot also automatically configures beans that support Thymeleaf views in Spring MVC. This happens because we used @Grab to add H2 and Thymeleaf to the classpath, which triggers auto-configuration for an embedded H2 database and Thymeleaf.

The @Grab annotation is an easy way to add dependencies that the CLI isn’t able to automatically resolve. In spite of its ease of use, however, there’s more to this little annotation than meets the eye. Let’s take a closer look at @Grab to see what makes it tick, how the Spring Boot CLI makes it even easier by requiring only an artifact name for many commonly used dependencies, and how to configure its dependency-resolution process.
