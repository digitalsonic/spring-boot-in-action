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

The Java language has seen great improvements in recent versions. Even so, Java has a few strict rules that add noise to the code. Line-ending semicolons, class and method modifiers (such as public and private), getter and setter methods, and import statements serve a purpose in Java, but they distract from the essentials of the code. From a developer’s perspective, code noise is friction—friction when writing code and even more friction when trying to read it. If some of this code noise could be eliminated, it’d be easier to develop and read the code.

Likewise, build systems such as Maven and Gradle serve a purpose in a project. But build specifications are also one more thing that must be developed and maintained. If only there were a way to do away with the build, projects would be that much simpler.

When working with the Spring Boot CLI, there is no build specification. The code itself serves as the build specification, providing hints that guide the CLI in resolving dependencies and producing deployment artifacts. Moreover, by teaming up with Groovy, the Spring Boot CLI offers a development model that eliminates almost all code noise, producing a friction-free development experience.

In the very simplest case, writing a CLI-based application could be as easy as writing a single standalone Groovy script like the one we wrote in chapter 1. But when writing a more complete application with the CLI, it makes sense to set up a basic project structure to house the project code. That’s where we’ll get started with rewriting the reading-list application.

### 5.1.1 Setting up the CLI project

The first thing we’ll do is create a directory structure to house the project. Unlike Maven- and Gradle-based projects, Spring Boot CLI projects don’t have a strict project structure. In fact, the simplest Spring Boot CLI app could be a single Groovy script living in any directory in the filesystem. For the reading-list project, however, you should create a fresh, clean directory to keep the code separate from anything else you keep on your machine:

```
$ mkdir readinglist
```

Here I’ve named the directory “readinglist”, but feel free to name it however you wish. The name isn’t as important as the fact that the project will have a place to live.

We’ll need a couple of extra directories to hold the static web content and the Thymeleaf template. Within the readinglist directory, create two new directories
named “static” and “templates”:

```
$ cd readinglist
$ mkdir static
$ mkdir templates
```

It’s no coincidence that these directories are named the same as the directories we created under src/main/resources in the Java-based project. Although Spring Boot doesn’t enforce a project structure like Maven and Gradle do, Spring Boot will still auto-configure a Spring ResourceHttpRequestHandler that looks for static content in a directory named “static” (among other locations). It will also still configure Thymeleaf to resolve templates from a directory named “templates”.

Speaking of static content and Thymeleaf templates, those files will be exactly the same as the ones we created in chapter 2. So that you don’t have to worry about remembering them later, go ahead and copy style.css into the static directory and readingList.html into templates.

At this point the reading-list project should be structured like this:

```
.
|---static
|   |---style.css
|---templates
    |---readingList.html
```

Now that the project is set up, we’re ready to start writing some Groovy code.
