# Testing with Spring Boot
# Spring Boot测试

__This chapter covers__  
__本章内容涉及__

* Integration testing
* Testing apps in a server
* Spring Boot’s test utilities
* 集成测试
* 在服务器里测试应用程序
* Spring Boot的测试辅助工具

It’s been said that if you don’t know where you’re going, any road will get you there. But with software development, if you don’t know where you’re going, you’ll likely end up with a buggy application that nobody can use.  
有人说，如果你不知道要去哪，走就是了。但在软件开发领域里，如果你都不知道要去哪里，那结果往往是做出一个满是Bug的应用程序，没人能用的了。

The best way to know for sure where you’re going when writing applications is to write tests that assert the desired behavior of an application. If those tests fail, you know you have some work to do. If they pass, then you’ve arrived (at least until you think of some more tests that you can write).  
在编写应用程序时，搞明白自己在干嘛的最佳途径就是写点测试，确定应用程序的行为符合预期。如果测试失败了，你就有活要干了。如果测试通过了，那你就到点了（至少在你觉得还有其他测试要写之前）。

Whether you write tests first or after the code has already been written, it’s important that you write tests to not only verify the accuracy of your code, but to also to make sure it does everything you expect it to. Tests are also a great safeguard to make sure that things don’t break as your application continues to evolve.  
究竟是在编写业务代码之前写测试，还是之后写并不重要，重要的是你写测试的目的并不仅仅是验证代码的准确性，还要确认它符合你的预期。测试也是一道保障，确认应用程序在进化的同时不会破坏已有的东西。

When it comes to writing unit tests, Spring is generally out of the picture. Loose coupling and interface-driven design, which Spring encourages, makes it really easy to write unit tests. But Spring isn’t necessarily involved in those unit tests.  
在编写单元测试的时候，Spring通常并不需要介入。Spring鼓励松耦合、接口驱动的设计，这些都让你能很轻松地编写单元测试，而且在写单元测试时并不需要用到Spring。

Integration tests, on the other hand, require some help from Spring. If Spring is responsible for configuring and wiring up the components in your production application, then Spring should also be responsible for configuring and wiring up those components in your tests.  
但在另一方面，集成测试就要用到Spring了。如果你的生产应用程序使用Spring来配置并组装组件，那么在你的测试里同样需要让它来配置并组装那些组件。

Spring’s SpringJUnit4ClassRunner helps load a Spring application context in JUnit-based application tests. Spring Boot builds on Spring’s integration testing support by enabling auto-configuration and web server startup when testing Spring Boot applications. It also offers a handful of useful testing utilities.  
Spring的`SpringJUnit4ClassRunner`可以帮你在基于JUnit的应用程序测试里加载Spring应用程序上下文。在测试Spring Boot应用程序时，Spring Boot除了拥有Spring的集成测试支持，还开启了自动配置和Web服务器，并提供了不少有用的测试辅助工具。

In this chapter, we’ll look at all of the ways that Spring Boot supports integration testing. We’ll start by looking at how to test with a fully Spring Boot-enabled application context.  
在本章中，我们会看到Spring Boot的各种集成测试支持。让我们先来看看如何在Spring Boot应用程序上下文里做测试。

## 4.1 Integration testing auto-configuration

At the core of everything that the Spring Framework does, its most essential task is to wire together all of the components that make up an application. It does this by read- ing a wiring specification (whether it be XML, Java-based, Groovy-based, or otherwise), instantiating beans in an application context, and injecting beans into other beans that depend on them.

When integration testing a Spring application, it’s important to let Spring wire up the beans that are the target of the test the same way it wires up those beans when the application is running in production. Sure, you might be able to manually instantiate the components and inject them into each other, but for any substantially big application, that can be an arduous task. Moreover, Spring offers additional facilities such as component-scanning, autowiring, and declarative aspects such as caching, transactions, and security. Given all that would be required to recreate what Spring does, it’s generally best to let Spring do the heavy lifting, even in an integration test.

Spring has offered excellent support for integration testing since version 1.1.1. Since Spring 2.5, integration testing support has been offered in the form of SpringJUnit4ClassRunner, a JUnit class runner that loads a Spring application context for use in a JUnit test and enables autowiring of beans into the test class.

For example, consider the following listing, which shows a very basic Spring integration test.
