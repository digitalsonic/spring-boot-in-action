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
One difference you may have noticed between this Groovy implementation of Book and the Java implementation in chapter 2 is that there are no JPA annotations. That’s because this time we’re going to use Spring’s JdbcTemplate to access the database instead of Spring Data JPA.

>There are a couple of very good reasons why I chose JDBC instead of JPA for this exam- ple. First, by mixing things up a little, I can show off a few more auto-configuration tricks that Spring Boot performs when working with Spring’s JdbcTemplate. But per- haps the most important reason I chose JDBC is that Spring Data JPA requires a .class file when generating on-the-fly implementations of repository interfaces. When you run Groovy scripts via the command line, the CLI compiles the scripts in memory and doesn’t produce .class files. Therefore, Spring Data JPA doesn’t work well when running scripts through the CLI.

>That said, the CLI isn’t completely incompatible with Spring Data JPA. If you use the CLI’s jar command to package your application into a JAR file, the resulting JAR file will contain compiled .class files for all of your Groovy scripts. Building and running a JAR file from the CLI is a handy option when you want to deploy an application devel- oped with the CLI, but it isn’t as convenient during development when you want to see the results of your work quickly.

Now that we’ve defined the Book domain class, let’s write the repository. First, let’s write the ReadingListRepository interface (in ReadingListRepository.groovy):

```
interface ReadingListRepository {

    List<Book> findByReader(String reader)

    void save(Book book)

}
```

Aside from a clear lack of semicolons and no public modifier on the interface, the Groovy version of ReadingListRepository isn’t much different from its Java counter- part. The most significant difference is that it doesn’t extend JpaRepository because we’re not working with Spring Data JPA in this chapter. And since we’re not using Spring Data JPA, we’re going to have to write the implementation of ReadingListRepository ourselves. The following listing shows what JdbcReadingListRepository.groovy should look like.

__Listing 5.1 A Groovy and JDBC implementation of ReadingListRepository__

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

RowMapper closure

For the most part, this is a typical JdbcTemplate-based repository implementation. It’s injected, via autowiring, with a reference to a JdbcTemplate object that it uses to query the database for books (in the findByReader() method) and to save books to the database (in the save() method).

By writing it in Groovy, we’re able to apply some Groovy idioms in the implementation. For example, in findByReader(), a Groovy closure is given as a parameter in the call to query() in place of a RowMapper implementation.1 Also, within the closure, a new Book object is created using Groovy’s support for setting object properties at construction.

While we’re thinking about database persistence, we also need to create a file named schema.sql that will contain the SQL necessary to create the Book table that the repository issues queries against:

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

[1]: # "In fairness to Java, we can do something similar in Java 8 using lambdas (and method references)."
