# appendix A Spring Boot Developer Tools
# 附录A Spring Boot开发者工具

Spring Boot 1.3 introduced a new set of developer tools that make it even easier to work with Spring Boot at development time. Among its many capabilities are  
Spring Boot 1.3引入了一组新的开发者工具，让你在开发时能更方便地使用Spring Boot。其功能包括

* Automatic restart—Restarts a running application when files are changed in the classpath
* LiveReload support—Changes to resources trigger a browser refresh automatically
* Remote development—Supports automatic restart and LiveReload when deployed remotely
* Development property defaults—Provides sensible development defaults for some configuration properties
* 自动重启——当Classpath里的文件发生变化时自动重启运行中的应用程序
* LiveReload支持——对资源的修改自动触发浏览器刷新
* 远程开发——远程部署时支持自动重启和LiveReload
* 默认的开发时属性值——为一些属性提供有意义的默认开发时属性值

Spring Boot’s developer tools come in the form of a library that can be added to a project as a dependency. If you’re using Gradle to build your project, the development tools can be added with the following line in your build.gradle file:  
Spring Boot的开发者工具是一个可以加入项目的依赖，如果你使用Gradle来构建项目，可以像下面这样在build.gradle文件里添加开发工具：

```
compile "org.springframework.boot:spring-boot-devtools"
```

Or it can be added as a <dependency> in a Maven POM:  
在Maven POM里添加`<dependency>`是这样的：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

The developer tools will be disabled when your application is running from a fully packaged JAR or WAR file, so it’s unnecessary to remove this dependency before building a production deployment.  
当应用程序以完整打包好的JAR或WAR文件形式运行时，开发者工具会被禁用，所以没有必要在构建生产部署包前移除这个依赖。

## Automatic restart
## 自动重启

With the developer tools active, any changes to files on the classpath will trigger an application restart. To make the restart as fast as possible, classes that won’t change (such as those in third-party JAR files) will be loaded into a base classloader, whereas application code that is being worked on will be loaded into a separate restart classloader. When changes are detected, only the restart classloader is restarted.  
在激活了开发者工具后，Classpath里对文件做任何修改都会触发应用程序重启。为了让重启速度够快，不会修改的类（比如第三方JAR文件里的）都被加载到基础类加载器里，而应用程序的代码则会被加载到一个单独的重启类加载器里。当检测到变更时，只有重启类加载器才会被重启。

There are some classpath resources that don’t require an application restart when they change. View templates, such as Thymeleaf templates, can be edited on the fly without restarting the application. Static resources in /static or /public likewise don’t require an application restart, so Spring Boot developer tools exclude the following paths from restart consideration: /META-INF/resources, /resources, /static, /public, /templates.  
有些Classpath里的资源变更后不需要重启应用程序。像Thymeleaf这样的视图模板可以直接编辑，不用重启应用程序。在/static或/public里的静态资源也不用重启应用程序，所以Spring Boot开发者工具会在重启时排除掉如下目录：/META-INF/resources、/resources、/static、/public和/templates。

The default set of restart path exclusions can be overridden by setting the spring.devtools.restart.exclude property. For example, to only exclude /static and /templates, set spring.devtools.restart.exclude like this:  
可以设置`spring.devtools.restart.exclude`属性来覆盖默认的重启排除目录。例如，你只要排除/static和/templates目录，可以像这样设置`spring.devtools.restart.exclude`：

```
spring:
  devtools:
    restart:
      exclude: /static/**,/templates/**
```

On the other hand, if you’d rather disable automatic restart completely, you can set spring.devtools.restart.enabled to false:  
另一方面，如果你想彻底关闭自动重启，可以将`spring.devtools.restart.enabled`设置为`false`：

```
spring:
  devtools:
    restart:
      enabled: false
```

Another option is to set a trigger file that must be changed in order for the restart to take place. For example, suppose you don’t want a restart to happen unless a change is made to a file named .trigger. All you must do is set the spring.devtools.restart.trigger-file property like this:  
另外还有一个选项，设置一个触发文件，必须修改这个文件才能触发重启。例如，在修改名为.trigger的文件前你都不希望执行重启，你只需像这样设置`spring.devtools.restart.trigger-file`属性：

```
spring:
  devtools:
    restart:
      trigger-file: .trigger
```

A trigger file is useful if your IDE continuously compiles changed files. Without a trigger file, every change would trigger a restart. With a trigger file, you can be sure that a restart doesn’t happen unless you want it to (by making a change to the trigger file).  
如果你的IDE会连续编译修改的文件，那触发文件还是很有用的。没有触发文件的话，每次变更都会触发重启。有触发文件，就能保证只有你想重启时才会发生重启（修改触发文件即可）。

## LiveReload
## LiveReload

One of the most common rituals of web application development involves the following steps:  
在Web应用程序开发过程中最常见的步骤大概是这样的：

1. Make a change to rendered content (such as images, stylesheets, templates).
2. Click Refresh in the browser to see the results of the change.
3. Repeat starting at step 1.

1. 修改要呈现的内容（比如图片、样式表、模板）。
2. 点击浏览器里的刷新按钮查看修改的结果。
3. 回到第1步。

Although it’s not an arduous process, it would be nice if you could see the results of a change immediately, without clicking Refresh.
虽然这不是什么难事，但如果能不点刷新就直接看到修改结果那岂不是更好。

Spring Boot’s developer tools integrate with LiveReload (http://livereload.com) to eliminate the Refresh step. When the developer tools are active, Spring Boot will start an embedded LiveReload server that can trigger a browser refresh whenever a resource is changed. All you need to do is install the LiveReload plugin into your web browser.  
Spring Boot的开发者工具中集成了LiveReload（[http://livereload.com](http://livereload.com)），可以消除刷新的步骤。激活了开发者工具后，Spring Boot会启动一个内嵌的LiveReload服务器，在资源文件变化时会触发浏览器刷新。你要做的就是在浏览器里安装LiveReload插件。

If you’d like to disable the embedded LiveReload server, you can do so by setting spring.devtools.livereload.enabled to false:  
如果想要禁用内嵌的LiveReload服务器，可以将`spring.devtools.livereload.enabled`设置为`false`：

```
spring:
  devtools:
    livereload:
      enabled: false
```

## Remote development
## 远程开发

The automatic restart and LiveReload features of the developer tools are also optionally available when running the applications remotely (such as when deployed on a server or in a cloud environment). In addition, Spring Boot’s developer tools enable remote debugging of Spring Boot applications.  
在远程运行应用程序时（比如部署到服务器上或云上），开发者工具的自动重启和LiveReload特性都是可选的。此外，Spring Boot开发者工具还能远程调试Spring Boot应用程序。

In a typical deployment, you won’t want the remote development feature enabled, as it will hinder performance. But in special cases, such as when developing an application that’s deployed in a non-production environment set aside for development purposes, these tools can come in handy. This is especially useful if your application uses a cloud service that isn’t available in your local development environment.  
在传统的开发过程中，你不会打开远程开发功能，因为这会影响性能。但在一些特殊的场景中，比如，出于开发目的，所开发的应用程序部署在非生产环境里，这时此类工具就很有用了，尤其是你的应用程序不在本地开发环境里，而是部署在云端时。

You must opt in to remote development by setting a remote secret:  
你必须设置一个远程安全码来开启远程开发功能：

```
spring:
  devtools:
    remote:
      secret: myappsecret
```

By setting this property, a server component is enabled in the running application to support remote development. This server will listen for requests asking it to accept incoming changes and will either restart the application or trigger a browser refresh.  
有了这个属性后，运行中的应用程序就会启动一个服务器组件来支持远程开发，它会监听接受变更的请求，可以重启应用程序或者触发浏览器刷新。

In order to put this remote server to use, you’ll need to run the remote development tools client locally. The remote client comes in the form of a class whose fully qualified name is org.springframework.boot.devtools.RemoteSpringApplication. It’s designed to run in your IDE with an argument telling it where your remote application is deployed.  

为了使用这个远程服务器，你需要在本地运行远程开发工具的客户端。这个远程客户端是一个类，全限定类名是`org.springframework.boot.devtools.RemoteSpringApplication`。它会运行在你的IDE里，要求提供一个参数，告诉它远程应用程序部署在哪里。

For example, suppose you’re running the reading-list application remotely, deployed on Cloud Foundry at https://readinglist.cfapps.io. If you’re using Eclipse or Spring ToolSuite, you can start the remote client with the following steps:  
例如，假设你正远程运行阅读列表应用程序，部署在Cloud Foundry上，地址是[https://readinglist.cfapps.io](https://readinglist.cfapps.io)。如果你在用Eclipse或Spring ToolSuite，可以通过如下步骤开启远程客户端：

1. Select the Run > Run Configurations menu item.
2. Create a new Java Application launch configuration.
3. Select the Reading List project in the Project field (either by typing the project name or clicking the Browse button and finding it). See figure A.1.
4. Enter org.springframework.boot.devtools.RemoteSpringApplication into the Main Class field. See figure A.1.
5. On the Arguments tab, enter https://readinglist.cfapps.io into the Program Arguments field. See figure A.2.

1. 选择Run > Run Configurations菜单项
2. 创建一个新的Java Application运行配置
3. 在Project里选中Reading List项目（可以键入项目名或者点击Browse按钮找到这个项目）。见图A.1。
4. 在Main Class里键入`org.springframework.boot.devtools.RemoteSpringApplication`。见图A.1。
5. 切换到Arguments标签页，在Program Arguments里键入`https://readinglist.cfapps.io`。见图A.2。

![图A.1](../Figures/figure-A.1.png)

__Figure A.1 RemoteSpringApplication is the remote developer tools client.__  
__图A.1 `RemoteSpringApplication`是远程开发者工具客户端。__

![图A.2](../Figures/figure-A.2.png)

__Figure A.2 RemoteSpringApplication takes the remote app’s URL as an argument.__  
__图A.2 `RemoteSpringApplication`将远程应用程序的URL作为参数。__

Once the client has started, you can start making changes to the application in your IDE. As changes are detected, they’ll be pushed to the remote server and applied. If changes are made to a rendered web resource (such as a stylesheet or JavaScript), they’ll also trigger a browser refresh using LiveReload.  
客户端启动后，就可以在IDE里修改应用程序了。在检测到变动后，这些修改点会被推送到远端并加以应用。如果修改的内容涉及呈现的Web资源（比如样式表或JavaScript），则还会用LiveReload触发浏览器刷新。

The remote client will also enable tunneling of remote debug traffic over HTTP so that you can debug a remotely deployed application in your IDE. All you must do is ensure that the remote application has remote debugging enabled. This can usually be done by configuring JAVA_OPTS.  
远程客户端还会开启基于HTTP的远程调试通道，这样就能在IDE里调试部署在远程的应用程序了。你要做的就是确保远程应用程序打开了远程调试功能。通常可以通过配置`JAVA_OPTS`来实现。

For example, if your application is deployed to Cloud Foundry, you can set JAVA_OPTS in your application’s manifest.yml file like this:  
比方说，你的应用程序部署在Cloud Foundry上，可以像下面这样在应用程序的manifest.yml里设置`JAVA_OPTS`：

```
---
  env:
    JAVA_OPTS: "-Xdebug -Xrunjdwp:server=y,transport=dt_socket,suspend=n"
```

Once the remote application is started and a connection is established with the local debug server, you should be able to set breakpoints and step through the code of the remote application much as if it were local (albeit a bit slower due to network latency).  
远程应用程序启动后，就会和本地调试服务器建立一个连接，你可以设置断点，一步步执行远程应用程序里的代码，就好像它们是运行在本地的一样（出于网络原因，速度会有点慢）。

## Development property defaults
## 默认的开发时属性

There are some configuration properties that are usually set at development time, but never in a production setting. View template caching, for instance, is best disabled during development so that you can see the results of any changes you make immediately. But in production, view template caching should be left enabled for better performance.  
有些配置属性通常是在开发时设置的，从来不用在生产环境里。比如视图模板缓存，在开发时最好关掉，这样你可以立刻看到修改的结果。但在生产环境里，为了追求更好的性能，应该开启视图模版缓存。

By default, Spring Boot will enable caching for any of the supported view template options (Thymeleaf, Freemarker, Velocity, Mustache, and Groovy templates). But if Spring Boot’s developer tools are in play, that caching will be disabled.  
默认情况下，Spring Boot会为其支持的各种视图模板（Thymeleaf、Freemarker、Velocity、Mustache和Groovy模板）开启缓存选项。但如果存在Spring Boot的开发者工具，就会禁用这些缓存。

Essentially what this means is that when the developer tools are active, the following properties are set to false:  
实际也就是说在开发者工具激活后，如下属性会被设置为`false`：

* spring.thymeleaf.cache
* spring.freemarker.cache
* spring.velocity.cache
* spring.mustache.cache
* spring.groovy.template.cache

This saves you from having to disable them (likely in a development-profiled configuration) for development time.  
这就不用你在开发时自己去禁用它们了（比如在一个开发时使用的Profile配置里）。

## Globally configuring developer tools
## 全局配置开发者工具

As you work with the developer tools, you’ll probably find that you regularly use the same settings across multiple projects. For instance, if you use a restart trigger file, you’re likely to name the trigger file consistently across projects. Rather than repeat developer tool configuration in each project, it may be more convenient to configure the developer tools globally.  
在使用开发者工具时，你应该已经注意到了，你通常在多个项目里都会使用相同的设置。举个例子，如果你使用了重启触发文件，那么你很可能在多个项目里都使用相同的触发文件名。相比在每个项目里重复开发者工具配置，对开发者工具做全局配置显得更方便一些。

To do this, create a file named .spring-boot-devtools.properties in your home directory. (Note that the name starts with a period.) In that file, set whatever developer tool properties you want to have applied across all of your projects.  
要实现这个目的，可以在你的主目录（home directory）里创建一个名为.spring-boot-devtools.properties的文件。（请注意，文件名用点开头。）在那个文件里，可以设置各种你希望在多个项目里共享的开发者工具属性。

For example, suppose that you want to set a trigger file named .trigger and disable LiveReload across all of your Spring Boot projects. To do that, you can create a .spring-boot-devtools.properties file with the following lines:  
例如，假设你想把触发文件的名称设置为.trigger，在所有Spring Boot项目里禁用LiveReload。可以创建一个.spring-boot-devtools.properties文件，包含如下内容：

```
spring.devtools.restart.trigger-file=.trigger
spring.devtools.livereload.enabled=false
```

Then, should you want to override any of these properties, you can do so on a project-by-project basis by setting them in each project’s application.properties or application.yml file.  
要是你想覆盖这些配置，可以在每个项目的application.properties或application.yml文件里设置特定于每个项目的属性。
