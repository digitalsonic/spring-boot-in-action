# foreword

# 推荐序

In the spring of 2014, the Delivery Engineering team at Netflix set out to achieve a lofty goal: enable end-to-end global continuous delivery via a software platform that facilitates both extensibility and resiliency. My team had previously built two different applications attempting to address Netflix's delivery and deployment needs, but both were beginning to show the telltale signs of monolith-ness and neither met the goals of flexibility and resiliency. What's more, the most stymieing effect of these monolithic applications was ultimately that we were unable to keep pace with our partner's innovation. Users had begun to move around our tools rather than with them.  
2014年春天，Netflix的交付工程团队（Delivery Engineering team）开始着手实现一个伟大的目标——通过一个软件平台来实现端到端的全局持续交付，该平台有利于系统的可扩展性及系统的弹性。为了满足Netflix的交付与部署需要，我的团队曾构建了两套不同的应用程序，但这两套应用程序都有演变成庞然大物的趋势，而且都没能满足灵活性和弹性的目标。更重要的是，这些庞大的应用程序最终还拖了我们的后腿，让我们跟不上合作伙伴创新的步伐。用户们开始规避我们的工具，而不是使用它们。

It became apparent that if we wanted to provide real value to the company and rapidly innovate, we needed to break up the monoliths into small, independent services that could be released at will. Embracing a microservice architecture gave us hope that we could also address the twin goals of flexibility and resiliency. But we needed to do it on a credible foundation where we could count on real concurrency, legitimate monitoring, reliable and easy service discovery, and great runtime performance.  
很明显，如果我们想要向公司证明自己的真正价值并快速创新，我们需要把庞然大物打散成小的独立服务，这些服务要能随时发布。拥抱微服务架构给我们带来了希望，让我们能实现灵活性与弹性的双重目标。但是我们需要在一个可靠的基础上实现这一架构，它要能做到真正的并发、合理的监控、可靠易用的服务发现，运行时还要有极好的性能。

With the JVM as our bedrock, we looked for a framework that would give us rapid velocity and steadfast operationalization out of the box. We zeroed in on Spring Boot.  
我们要在JVM上寻找一款框架，它要能直接提供快速开发和强大运维能力。最终，我们找到了Spring Boot。

Spring Boot makes it effortless to create Spring-powered, production-ready services without a lot of code! Indeed, the fact that a simple Spring Boot Hello World application can fit into a tweet is a radical departure from what the same functionality required on the JVM only a few short years ago. Out-of-the-box nonfunctional features like security, metrics, health-checks, embedded servers, and externalized configuration made Boot an easy choice for us.  
Spring Boot能在寥寥数行代码下构建起一套基于Spring并满足生产要求的服务，一切得来不费吹灰之力！实际上，一个简单的Spring Boot Hello World应用程序能放进一条推文里，这在短短几年之前还是完全不可能的事情。它还自带了不少非功能性的特性，比如安全、度量、健康检查、内嵌服务器和外置配置，这些都让选择Spring Boot成为了一件顺理成章的事情。

Yet, when we embarked on our Spring Boot journey, solid documentation was hard to come by. Relying on source code isn’t the most joyful manner of figuring out how to properly leverage a framework’s features.  
然而，当我们踏上Spring Boot之旅后却发现手头没有好的文档。要搞明白怎么利用好框架的特性，只能依托于源码，这可不是个让人愉快的办法。

It’s not surprising to see the author of Manning’s venerable Spring in Action take on the challenge of concisely distilling the core aspects of working with Spring Boot into another cogent book. Nor is it surprising that Craig and the Manning crew have done another tremendously wonderful job! Spring Boot in Action is an easily readable book, as we’ve now come to expect from Craig and Manning.  
Manning那本著名的___《Spring in Action》___的作者再度接受挑战，将Spring Boot的核心用法写成了另一部巨著，得知这一消息后我一点都不吃惊，毫无疑问，Craig和Manning的团队又做成了一件了不起的大事！正如我们所料，___《Spring Boot in Action》___是一本通俗易懂的好书。

From chapter 1’s attention-getting introduction to Boot and the now legendary 90ish-character tweetable Boot application to an in-depth analysis of Boot’s Actuator in chapter 7, which enables a host of auto-magical operational features required for any production application, Spring Boot in Action leaves no stone unturned. Indeed, for me, chapter 7’s deep dive into the Actuator answered some of the lingering questions I’ve had in the back of my head since picking up Boot well over a year ago. Chapter 8’s thorough examination of deployment options opened my eyes to the simplicity of Cloud Foundry for cloud deployments. One of my favorite chapters is chapter 4, where Craig explores the many powerful options for easily testing a Boot application. From the getgo, I was pleasantly surprised with some of Spring’s testing features, and Boot takes advantage of them nicely.  
从第1章那引人入胜的介绍以及富有传奇色彩的90字符推文应用程序，一直到第7章里对Spring Boot的Actuator（它提供了很多生产应用程序所需的神奇的运维特性）的深度分析，___《Spring Boot in Action》___做到了知无不言，言无不尽。实际上，对我而言，第7章里对Actuator的深度分析解答了我的不少问题，这些问题自一年多以前我开始使用Spring Boot之后就一直萦绕在我的脑海里。第8章里对部署选项的彻底研究让我大开眼界，了解到了Cloud Foundry在云部署方面是如此简便。第4章是我最喜欢的章节之一，Craig揭示了很多强大的选项，它们能很方便地测试一个Spring Boot应用程序。打一开始我就惊讶于Spring的测试特性，而Spring Boot将它们发挥的淋漓尽致。

As I’ve publicly stated before, Spring Boot is just the kind of framework the Java community has been seeking for over a decade. Its easy-to-use development features and out-of-the-box operationalization make Java development fun again. I’m pleased to report that Spring and Spring Boot are the foundation of Netflix’s new continuous delivery platform. What’s more, other teams at Netflix are following the same path because they too see the myriad benefits of Boot.  
正如上文中我所说的那样，Spring Boot正是十几年来Java社区所探寻的那种框架。它那简单易用的开发特性和开箱即用（out-of-the-box）的运维能力让Java开发又有趣起来了。我很高兴地向大家宣布Spring和Spring Boot已经成为了Netflix新持续交付平台的基础。而且，Netflix的其他团队也参考了我们的做法，因为他们也看到了Spring Boot是极其有益的。

It’s with equal parts excitement and passion that I absolutely endorse Craig’s book as the easy-to-digest and fun-to-read Spring Boot documentation the Java community has been waiting for since Boot took the community by storm. Craig’s accessible writing style and sweeping analysis of Boot’s core features and functionality will surely leave readers with a solid grasp of Boot (along with a joyful sense of awe for it).  
我怀着兴奋与激动的心情，向大家强烈推荐Craig的书，作为Spring Boot的文档，本书可谓通俗易懂、趣味横生，是Spring Boot征服Java社区后大家翘首以盼的佳作。Craig浅显易懂的写作风格，对Spring Boot核心特性与功能的全面分析，一定能让读者对Spring Boot有个彻底的认识（同时还伴随着一种喜悦与肃然起敬交织在一起的心情）。

Keep up the great work Craig, Manning Publications, and all the brilliant developers who have made Spring Boot what it is today! Each one of you has ensured a bright future for the JVM.  
Craig加油！Manning出版社加油！那些开发出Spring Boot的天才开发者们加油！请你们一定坚持下去！就是你们确保了JVM能有一个光明的未来。

ANDREW GLOVER  
MANAGER, DELIVERY ENGINEERING AT NETFLIX  
Netflix交付工程团队经理

# preface

# 前言

At the 1964 New York World’s Fair, Walt Disney introduced three groundbreaking attractions: “it’s a small world,” “Great Moments with Mr. Lincoln,” and the “Carousel of Progress.” All three of these attractions have since moved into Disneyland and Walt Disney World, and you can still see them today.  
在1964年的纽约世界博览会（New York World’s Fair）上，Walt Disney向世界介绍了三件有开创意义的东西：“小小世界”（it's a small world）、“与林肯先生共度的伟大时刻”（Great Moments with Mr. Lincoln）以及“文明演进之旋转木马”（Carousel of Progress）<sup>【[译注1][P1]】</sup>。所有这三样东西随后都被搬进了迪士尼乐园（Disneyland）和迪士尼世界（Walt Disney World），你今天仍能看见它们。

My favorite of these is the Carousel of Progress. Supposedly, it was one of Walt Disney’s favorites too. It’s part ride and part stage show where the seating area rotates around a center area featuring four stages. Each stage tells the story of a family at different time periods of the 20th century—the early 1900s, the 1920s, the 1940s, and recent times—highlighting the technology advances in that time period. The story of innovation is told from a hand-cranked washing machine, to electric lighting and radio, to automatic dishwashers and television, to computers and voice-activated appliances.  
其中，我最喜欢的是文明演进之旋转木马，恐怕这也是Walt Disney的最爱之一。它一部分是骑行，一部分是舞台演出，座位区域围绕着中心区域旋转，上演四场表演，讲述了一个家庭在20世纪不同时代的故事，分别是1900年代早期、1920年代、1940年代和近代，突出了那个年代的技术进步。从手摇洗衣机，到电灯和收音机，到自动洗碗机和电视，再到电脑和声控家电，无一不在述说着创新的故事。<sup>【[译注2][P2]】</sup>

In every act, the father (who is also the narrator of the show) talks about the latest inventions and says “It can’t get any better,” only to discover that, in fact, it does get better in the next act as technology progresses.  
在每幕表演中，父亲（也是演出的叙述者）都会讲述最新的发明，并带上一句“这玩意儿不能更好了（It can’t get any better）”，到头来却发现随着技术的进步，它的确变得更好了。

Although Spring doesn’t have quite as long a history as that displayed in the Carousel of Progress, I feel the same way about Spring as “Progress Dad” felt about the 20th century. Each and every Spring application seems to make the lives of developers so much better. Just looking at how Spring components are declared and wired together, we can see the following progression over the history of Spring:  
比起这场舞台演出，Spring的历史要短的多，但是关于Spring，我还是和“演进老爹”（Progress Dad）对20世纪的体会有类似的感受。看起来每个Spring应用程序都让开发者的生活更上一个台阶，仅从Spring组件的声明和织入方式就能看出端倪，让我们来看看Spring历史中的一些演化历程：

* When Spring 1.0 hit the scene, it completely changed how we develop enterprise Java applications. Spring dependency injection and declarative transactions meant no more tight coupling of components and no more heavyweight EJBs. It couldn’t get any better.  
Spring 1.0的出现彻底改变了我们开发企业级Java应用程序的方式。Spring的依赖注入与声明式事务意味着组件之间再也不存在紧耦合，可以再也不用重量级的EJB了。这玩意儿不能更好了。
* With Spring 2.0 we could use custom XML namespaces for configuration, making Spring itself even easier to use with smaller and easier to understand configuration files. It couldn’t get any better.  
到了Spring 2.0，我们可以在配置里使用自定义的XML命名空间，更小、更简单易懂的配置文件让Spring本身变得更便于使用。这玩意儿不能更好了。
* Spring 2.5 gave us a much more elegant annotation-oriented dependency-injection model with the @Component and @Autowired annotations, as well as an annotation-oriented Spring MVC programming model. No more explicit declaration of application components, and no more subclassing one of several base controller classes. It couldn’t get any better.  
Spring 2.5让我们有了更优雅的面向注解的依赖注入模型（即`@Component`和`@Autowired`注解），以及面向注解的Spring MVC编程模型。不用再去显式地声明应用程序组件了，也不再需要去继承某个基础的控制器类。这玩意儿不能更好了。
* Then with Spring 3.0 we were given a new Java-based configuration alternative to XML that was improved further in Spring 3.1 with a variety of @Enable-prefixed annotations. For the first time, it become realistic to write a complete Spring application with no XML configuration whatsoever. It couldn’t get any better.  
然后就到了Spring 3.0，我们有了一套全新的基于Java的配置，它能够取代XML。在Spring 3.1里，一系列以`@Enable`为前缀的注解进一步完善了这一特性。终于，第一次我们可以写出一个没有任何XML配置的Spring应用程序了。这玩意儿不能更好了。
* Spring 4.0 unleashed support for conditional configuration, where runtime decisions would determine which configuration would be used and which would be ignored based on the application’s classpath, environment, and other factors. We no longer needed to write scripts to make those decisions at build time and pick which configuration should be included in the deployment. How could it possibly get any better?  
Spring 4.0对条件配置（conditional configuration）提供了支持，根据应用程序的Classpath、环境和其他因素，运行时决策将决定使用哪些配置，忽略哪些配置。那些决策不再需要在构建时通过编写脚本确定了，以前会把选好的配置放在部署的包里，现在情况不同了。这玩意儿不能更好了。

And then came Spring Boot. Even though with each release of Spring we thought it couldn’t possibly get any better, Spring Boot proved that there’s still a lot of magic left in Spring. In fact, I believe Spring Boot is the most significant and exciting thing to happen in Java development in a long time.  
现在轮到Spring Boot了。虽然Spring的每个版本都让我们觉得一切都不能更好了，但Spring Boot还是向我们证明了Spring的魅力仍然势不可挡。事实上，我相信Spring Boot是长久以来Java开发历程里最意味深长和激动人心的东西。

Building upon previous advances in the Spring Framework, Spring Boot enables automatic configuration, making it possible for Spring to intelligently detect what kind of application you’re building and automatically configure the components necessary to support the application’s needs. There’s no need to write explicit configuration for common configuration scenarios; Spring will take care of it for you.  
在历代Spring Framework的基础之上，Spring Boot实现了自动配置，这让Spring能智能探测你正在构建何种应用程序，自动配置必要的组件来满足应用程序的需要。对于那些常见配置场景，不再需要显式地编写配置了，Spring会替你料理好一切的。

Spring Boot starter dependencies make it even easier to select which build-time and runtime libraries to include in your application builds by aggregating commonly needed dependencies. Spring Boot starters not only keep the dependencies section of your build specifications shorter, they keep you from having to think too hard about the specific libraries and versions you need.  
为了选择要将哪些构建时的库和运行时的库包含在应用程序里，往往要花费不少功夫，而Spring Boot的起步依赖（starter dependency）将常用依赖聚合在一起，藉此让一切都变简单了。它不仅简化了你的构建说明，还让你免受苦思冥想特定库和版本之苦。

Spring Boot’s command-line interface offers a compelling option for developing Spring applications in Groovy with minimal noise or ceremony common in Java applications. With the Spring Boot CLI, there’s no need for accessor methods, access modifiers such as public or private, semicolons, or the return keyword. In many cases, you can even eliminate import statements. And because you run the application as scripts from the command line, you don’t need a build specification.
针对使用Groovy来开发Spring应用程序，Spring Boot的命令行界面提供了一个引人瞩目的选项，它让Java应用程序开发过程中的噪音降到最低，开发方式平易近人。有了Spring Boot CLI，就不再需要存取方法（accessor methods），不再需要诸如`public`与`private`之类的访问修饰符（access modifiers），也不再需要分号或者`return`关键字。在一些场景中，`import`语句都可以去掉。因为你是在命令行里以脚本方式运行应用程序，连构建说明都能免了。

Spring Boot’s Actuator gives you insight into the inner workings of a running application. You can see exactly what beans are in the Spring application context, how Spring MVC controllers are mapped to paths, the configuration properties available to your application, and much more.  
Spring Boot的Actuator让你能一窥应用程序运行时的内部工作细节，看看Spring应用程序上下文里都有哪些Bean，Spring MVC控制器是怎么与路径互相映射的，应用程序都能取到哪些配置属性等等。

With all of these wonderful features enabled by Spring Boot, it certainly can’t get any better!  
Spring Boot为我们带来了这么多奇妙的特性，这玩意儿当然不能更好了！

In this book, you’ll see how Spring Boot has indeed made Spring even better than it was before. We’ll look at auto-configuration, Spring Boot starters, the Spring Boot CLI, and the Actuator. And we’ll tinker with the latest version of Grails, which is based on Spring Boot. By the time we’re done, you’ll probably be thinking that Spring couldn’t get any better.  
本书中你将看到Spring Boot着实让Spring比以前更好了，我们将一同去了解自动配置、Spring Boot起步依赖、Spring Boot CLI和Actuator。我们还会去摆弄下Grails的最新版本，它就是基于Spring Boot的。临近末尾，你也许会觉得Spring不可能更好了。

If we’ve learned anything from Walt Disney’s Carousel of Progress, it’s that when we think things can’t get any better, they inevitably do get better. Already, the advances offered by Spring Boot are being leveraged to enable even greater advances. It’s hard to imagine Spring getting any better than it is now, but it certainly will. With Spring, there’s always a great big beautiful tomorrow.  
如果说Walt Disney的文明演进之旋转木马告诉了我们什么事情，那就是当我们觉得什么东西不可能更好了的时候，它们一定会变得更好。之前Spring Boot提供的益处已经能撬动更大的利益了，现在真的很难想象Spring还能变得更好，但它显然能更好。毫无疑问，有Spring在手，总会有个美丽的明天。

[P1]: # "关于这届世博会里迪士尼相关的信息，详见http://www.dwz.cn/2Hrvyh中的Disney influence部分。"
[P2]: # "关于这个游乐设施，详见http://www.yesterland.com/progress.html的介绍。"

# about this book
# 关于本书

Spring Boot aims to simplify Spring development. As such, Spring Boot’s reach stretches to touch everything that Spring touches. It’d be impossible to write a book that covers every single way that Spring Boot can be used, as doing so would involve covering every single technology that Spring itself supports. Instead, Spring Boot in Action aims to distill Spring Boot into four main topics: auto-configuration, starter dependencies, the command-line interface, and the Actuator. Along the way, we’ll touch on a few Spring features as necessary, but the focus will be primarily on Spring Boot.  
Spring Boot旨在简化Spring的开发，就这点而论，Spring Boot涉及到了Spring的方方面面。你没办法通过一本书来讲清楚Spring Boot的每种用法，因为这样就必须涵盖Spring本身所支持的各种技术。所以___《Spring Boot in Action》___把Spring Boot大致分为四个主题：自动配置、起步依赖、命令行界面和Actuator。书中还会讲到一些必要的Spring特性，但主要精力还是集中在Spring Boot上。

Spring Boot in Action is for all Java developers. Although some background in Spring could be considered a prerequisite, Spring Boot has a way of making Spring more approachable even to those new to Spring. Nevertheless, because this book will be focused on Spring Boot and will not dive deeply into Spring itself, you may find it helpful to pair it with other Spring materials such as Spring in Action, Fourth Edition (Manning, 2014).  
___《Spring Boot in Action》___面向的是全体Java开发者。虽然会要求读者有一些Spring使用背景，但Spring Boot能让那些新接触Spring的人更容易上手。话虽如此，但本书的主要焦点还是在Spring Boot上，不会深入Spring本身，再手边准备一本其他Spring读物也许效果会更好一些，比如说___《Spring in Action》第四版___（Manning，2014）。

## Roadmap
## 章节安排

Spring Boot in Action is divided into seven chapters:  
___《Spring Boot in Action》___全书分为八个章节<sup>【[译注1][A1]】</sup>：

* In chapter 1 you’ll be given an overview of Spring Boot, including the essentials of automatic configuration, starter dependencies, the command-line interface, and the Actuator.  
第1章里会对Spring Boot做一个概述，包含了最基本的自动配置、起步依赖、命令行界面和Actuator。
* Chapter 2 takes a deeper dive into Spring Boot, focusing on automatic configuration and starter dependencies. In this chapter, you’ll build a complete Spring application using very little explicit configuration.  
第2章会进一步深入Spring Boot，聚焦于自动配置和起步依赖。在这一章里，你将用很少的显式配置来构建一个完整的Spring应用程序。
* Chapter 3 picks up where chapter 2 leaves off, showing how you can influence automatic configuration by setting application properties or completely overriding automatic configuration when it doesn’t meet your needs.  
第3章是对第2章的一个补充，演示了如何通过设置应用程序属性来改变自动配置，或者是在自动配置无法满足需要时彻底覆盖它。
* In chapter 4 we’ll look at how to write automated integration tests for Spring Boot applications.  
在第4章里我们会看到如何为Spring Boot应用程序编写自动化集成测试。
* In chapter 5 you’ll see how the Spring Boot CLI offers a compelling alternative to conventional Java development by enabling you to write complete applications as a set of Groovy scripts that are run from the command line.  
在第5章里你将看到一种有别于传统Java开发方式的做法，Spring Boot CLI让你能通过命令行来运行你写的应用程序，这个应用程序完全是由Groovy脚本构成的。
* While we’re on the subject of Groovy, chapter 6 takes a look at Grails 3, the latest version of the Grails framework, which is now based on Spring Boot.  
讲到了Groovy，第6章会介绍到Grails 3，这是Grails框架的最新版本，它就是基于Spring Boot的。
* In chapter 7 you’ll see how to leverage Spring Boot’s Actuator to dig inside of a running application and see what makes it tick. You’ll see how to use Actuator web endpoints as well as a remote shell and JMX MBeans to peek at the internals of an application.  
在第7章里你将看到如何通过Spring Boot的Actuator来了解运行中的应用程序，以及它是如何工作的。你还会看到如何使用Actuator的Web端点、远程Shell和JMX MBean对应用程序一窥究竟。
* Chapter 8 wraps things up by discussing various options for deploying your Spring Boot application, including traditional application server deployment and cloud deployment.  
第8章讨论了各种部署Spring Boot应用程序的方法，包括传统的应用程序服务器部署和云部署。

## Code conventions and downloads
## 编码规范及代码下载

There are many code examples throughout this book. These examples will always appear in a fixed-width code font like this. Any class name, method name, or XML fragment within the normal text of the book will appear in code font as well. Many of Spring’s classes and packages have exceptionally long (but expressive) names. Because of this, line-continuation markers (➥) may be included when necessary. Not all code examples in this book will be complete. Often I only show a method or two from a class to focus on a particular topic.  
全书包含了很多代码示例，这些代码使用了`类似这样的等宽字体`，正文中出现的所有类名、方法名或者是XML片段也会用这种字体。不少Spring的类和包的名字都特别长（但是很有表现力，一看就懂），为此在需要时会使用续行符（➥）。书中的代码并非都是完整的，通常我只会就某个特定主题摘出类中的一两个方法。

Complete source code for the applications found in the book can be downloaded from the publisher’s website at www.manning.com/books/spring-boot-in-action.
你可以在出版社的网站上下载到书中应用程序的完整代码，地址是[www.manning.com/books/spring-boot-in-action](www.manning.com/books/spring-boot-in-action)。

## Author Online
## 作者在线

The purchase of Spring Boot in Action includes free access to a private web forum run by Manning Publications, where you can make comments about the book, ask technical questions, and receive help from the author and from other users. To access the forum and subscribe to it, point your web browser to www.manning.com/books/spring-boot-in-action. This page provides information on how to get on the forum once you are registered, what kind of help is available, and the rules of conduct on the forum.  
购买本书还能免费访问Manning出版社的私有Web论坛，在那里你能就本书发表评论，询问技术问题，从作者以及其他用户处寻求帮助。如需访问并订阅该论坛，请打开浏览器访问[www.manning.com/books/spring-boot-in-action](www.manning.com/books/spring-boot-in-action)。该页面上提供了详细的信息，告诉你在注册后如何访问论坛，论坛里都能提供哪些帮助以及论坛的管理规则。

Manning’s commitment to our readers is to provide a venue where a meaningful dialogue between individual readers and between readers and the author can take place. It is not a commitment to any specific amount of participation on the part of the author whose contribution to the forum remains voluntary (and unpaid). We suggest you try asking the author some challenging questions lest his interest stray!  
Manning向读者承诺，为读者与读者之间，读者与作者之间的沟通建立一个桥梁。但Manning并不保证作者在论坛中的参与程度，他们在论坛上投入的精力都是自愿的（并且是无偿的）。我们强烈建议您向作者问些有挑战性的问题，让他有兴趣留在论坛里。

The Author Online forum and the archives of previous discussions will be accessible from the publisher’s website as long as the book is in print.  
只要本书仍在销售中，作者在线论坛及其讨论归档就会保留在出版商的网站上。

## About the cover illustration
## 关于封面插画

The figure on the cover of Spring Boot in Action is captioned “Habit of a Tartar in Kasan,” which is the capital city of the Republic of Tatarstan in Russia. The illustration is taken from Thomas Jefferys’ A Collection of the Dresses of Different Nations, Ancient and Modern (four volumes), London, published between 1757 and 1772. The title page states that these are hand-colored copperplate engravings, heightened with gum arabic. Thomas Jefferys (1719–1771) was called “Geographer to King George III.” He was an English cartographer who was the leading map supplier of his day. He engraved and printed maps for government and other official bodies and produced a wide range of commercial maps and atlases, especially of North America. His work as a mapmaker sparked an interest in local dress customs of the lands he surveyed and mapped, which are brilliantly displayed in this collection.  
___《Spring Boot in Action》___的封面插画题为《喀山鞑靼名族服饰》（Habit of a Tartar in Kasan），喀山是俄罗斯联邦鞑靼自治共和国首府。这副插画选自Thomas Jefferys的___《古今多名族服饰集》第四卷___（A Collection of the Dresses of Different Nations, Ancient and Modern），1757至1772年出版于伦敦，该书的扉页里说这些插画都是手工上色、铜版雕刻、还用了阿拉伯树胶。Thomas Jefferys（1719–1771）被誉为“乔治三世国王的御用地理学家”（Geographer to King George III）。他是一名英国地图制图师，是当时地图行业的领导者。他为政府和其他官方机构雕刻并印刷地图，还生产了各种不同的商用地图和地图集，尤其是北美洲的。地图制图师的工作引发了他对调研地的当地民族服饰的兴趣，这一兴趣在这本服饰集里体现的淋漓尽致。

Fascination with faraway lands and travel for pleasure were relatively new phenomena in the late eighteenth century, and collections such as this one were popular, introducing both the tourist as well as the armchair traveler to the inhabitants of other countries. The diversity of the drawings in Jefferys’ volumes speaks vividly of the uniqueness and individuality of the world’s nations some 200 years ago. Dress codes have changed since then, and the diversity by region and country, so rich at the time, has faded away. It is now often hard to tell the inhabitant of one continent from another. Perhaps, trying to view it optimistically, we have traded a cultural and visual diversity for a more varied personal life. Or a more varied and interesting intellectual and technical life.  
着迷于远方的大陆，为了消遣而去旅行，这在十八世纪晚期还是一个相对新鲜的现象，像这本服饰集这样的合集在当时非常流行，向观光客和足不出户的“游客”介绍其他国家的居民。Jefferys著作中各种各样的插画生动地述说了200年前世界上各个国家所独有的特色。自那以后，服饰文化已经发生了变化，那时地区与国家之间的差异如此丰富，这一切正在逐渐消失。现在往往很难分辨不同大洲的居民了。也许，我们该乐观一点，我们用文化和视觉上的多样性换来了更多样的人生，或者说是更多样、更有趣、更聪慧的科技人生。

At a time when it is hard to tell one computer book from another, Manning celebrates the inventiveness and initiative of the computer business with book covers based on the rich diversity of regional life of two centuries ago, brought back to life by Jeffreys’ pictures.  
在很难分辨出不同计算机读物的年代里，Manning脱颖而出，它所出版的图书封面体现了计算机行业别出心裁、独具创新的气质，靠的就是两个世纪以前各地居民丰富的多样性，这些都得归功于Jeffreys的插画。

[A1]: # "原文写的是seven chapters，估计是笔误。"

## acknowledgments
## 致谢

This book will show how Spring Boot can automatically deal with the behind-the-scenes stuff that goes into an application, freeing you to focus on the tasks that make your application unique. In many ways, this is analogous to what went into making this book happen. There were so many other people taking care of making things happen that I was free to focus on writing the content of the book. For taking care of the behind-the-scenes work at Manning, I’d like to thank Cynthia Kane, Robert Casazza, Andy Carroll, Corbin Collins, Kevin Sullivan, Mary Piergies, Janet Vail, Ozren Harlovic, and Candace Gillhoolley.  
本书将告诉你Spring Boot是如何自动处理应用程序幕后的各种杂事的，把你从这些琐事中解放出来，以便能聚焦在那些真正让它变得独一无二的事上。从很多方面来说，这和本书的诞生经历非常类似。很多人帮我操心了不少事情，让我能专心撰写本书的内容。比如Manning的各种事情，我就要感谢Cynthia Kane、Robert Casazza、Andy Carroll、Corbin Collins、Kevin Sullivan、Mary Piergies、Janet Vail、Ozren Harlovic以及Candace Gillhoolley。

Writing tests help you know if your software is meeting its goals. Similarly, those who reviewed Spring Boot in Action while it was still being written gave me the feedback I needed to make sure that the book stayed on target. For this, my gratitude goes out to Aykut Acikel, Bachir Chihani, Eric Kramer, Francesco Persico, Furkan Kamaci, Gregor Zurowski, Mario Arias, Michael A. Angelo, Mykel Alvis, Norbert Kuchenmeister, Phil Whiles, Raphael Villela, Sam Kreter, Travis Nelson, Wilfredo R. Ronsini Jr., and William Fly. Special thanks to John Guthrie for a final technical review shortly before the manuscript went into production. And extra special thanks to Andrew Glover for contributing the foreword to my book.  
编写测试能让你知道自己的软件是否实现了目标。同样的，在撰写过程中就为___《Spring Boot in Action》___审稿并提供反馈意见的各位，是你们让我确信本书没有偏离方向。为此，我要感谢Aykut Acikel、Bachir Chihani、Eric Kramer、Francesco Persico、Furkan Kamaci、Gregor Zurowski、Mario Arias、Michael A. Angelo、Mykel Alvis、Norbert Kuchenmeister、Phil Whiles、Raphael Villela、Sam Kreter、Travis Nelson、Wilfredo R. Ronsini Jr.以及William Fly。还要特别鸣谢John Guthrie在原稿即将付印前的最终技术审校。还要感谢Andrew Glover为本书所写的推荐序。

Of course, this book wouldn’t be possible or even necessary without the incredible work done by the talented members of the Spring team. It’s amazing what you do, and I’m so excited to be part of a team that’s changing how software is developed.  
当然，离开了Spring团队中各位天才成员的杰出工作，本书就不可能也不必问世了。你们太棒了，能成为改变软件开发方式的团队的成员，我的内心十分激动。

Many thanks to all of those involved in the No Fluff/Just Stuff tour, whether it be my fellow presenters or those who show up to hear us talk. The conversations we’ve had have in some small way contributed to how this book was formed.  
我还要感谢所有那些参加了No Fluff/Just Stuff活动<sup>【[译注2][A2]】</sup>的人们，无论是演讲嘉宾还是出席的听众，谢谢大家。我们之间的对话某种程度上也促成了本书。

A book like this would not be possible without an alphabet to compose into words. So, just as in my previous book, I’d like to take this opportunity to thank the Phoenicians for the invention of the first alphabet.  
没有那些组成文字的字母，像这样的书也不可能出现。因此，就和我之前的书一样，我想借此机会感谢发明第一个字母表的腓尼基人（the Phoenicians）。

Last, but certainly not least...my love, devotion, and thanks go to my beautiful wife Raymie and my awesome girls, Maisy and Madi. Once again, you’ve tolerated another writing project. Now that it’s done, we should go to Disney World. Whatdya say?  
最后，我要隆重感谢我的挚爱，我美丽的妻子Raymie，还有我了不起的女儿Maisy和Madi。你们又一次忍受住了我的写作项目。现在，书写完了，我们该去迪士尼世界了，你们说呢？

[A2]: # "这是一个面向JVM软件开发者的活动，详见https://nofluffjuststuff.com。"
