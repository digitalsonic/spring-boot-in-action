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
## 4.1 集成测试自动配置

At the core of everything that the Spring Framework does, its most essential task is to wire together all of the components that make up an application. It does this by reading a wiring specification (whether it be XML, Java-based, Groovy-based, or otherwise), instantiating beans in an application context, and injecting beans into other beans that depend on them.  
在Spring Framework的众多核心工作之中，最重要的就是讲所有组件编织在一起构成一个应用程序。整个过程就是读取配置说明（可以是XML、基于Java的配置、基于Groovy的配置或其他类型的配置），在应用程序上下文里初始化Bean，将Bean注入到依赖它们的其他Bean中。

When integration testing a Spring application, it’s important to let Spring wire up the beans that are the target of the test the same way it wires up those beans when the application is running in production. Sure, you might be able to manually instantiate the components and inject them into each other, but for any substantially big application, that can be an arduous task. Moreover, Spring offers additional facilities such as component-scanning, autowiring, and declarative aspects such as caching, transactions, and security. Given all that would be required to recreate what Spring does, it’s generally best to let Spring do the heavy lifting, even in an integration test.  
在对Spring应用程序进行集成测试时，让Spring用和生产环境一样的方式来组装那些测试目标Bean是非常重要的。当然，你也可以手动初始化组件，并将它们注入到其他组件里，但对那些大型应用程序来说，这是项费时费力的工作。而且，Spring提供了额外的辅助功能，比如组件扫描、自动织入和声明性切面（比如缓存、事务和安全）。你要把这些活都干了，基本也就是把Spring再造了一次，最好还是让Spring替你把重活都做了吧，哪怕是在集成测试里。

Spring has offered excellent support for integration testing since version 1.1.1. Since Spring 2.5, integration testing support has been offered in the form of SpringJUnit4ClassRunner, a JUnit class runner that loads a Spring application context for use in a JUnit test and enables autowiring of beans into the test class.  
Spring自1.1.1版就对集成测试提供了极佳的支持。自Spring 2.5开始，集成测试支持的形式就变成了`SpringJUnit4ClassRunner`，这是一个JUnit类运行器，他会为JUnit测试加载Spring应用程序上下文，并为测试类自动织入所需的Bean。

For example, consider the following listing, which shows a very basic Spring integration test.  
举例来说，看一下如下的代码，这是一个非常基本的Spring集成测试。

__Listing 4.1 Integration testing Spring with SpringJUnit4ClassRunner__  
__代码4.1 用`SpringJUnit4ClassRunner`对Spring应用程序进行集成测试__

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(
      classes=AddressBookConfiguration.class)
public class AddressServiceTests {

  @Autowired
  private AddressService addressService;

  @Test
  public void testService() {
    Address address = addressService.findByLastName("Sheman");
    assertEquals("P", address.getFirstName());
    assertEquals("Sherman", address.getLastName());
    assertEquals("42 Wallaby Way", address.getAddressLine1());
    assertEquals("Sydney", address.getCity());
    assertEquals("New South Wales", address.getState());
    assertEquals("2000", address.getPostCode());
  }
}

```
Loads application context  
加载应用程序上下文

Injects address service  
注入地址服务

Tests address service  
测试地址服务

As you can see, AddressServiceTests is annotated with both @RunWith and @ContextConfiguration. @RunWith is given SpringJUnit4ClassRunner.class to enable Spring integration testing.1 Meanwhile, @ContextConfiguration specifies how to load the application context. Here we’re asking it to load the Spring application context given the specification defined in AddressBookConfiguration.  
如你所见，`AddressServiceTests`上加注了`@RunWith`和`@ContextConfiguration`注解。`@RunWith`的参数是`SpringJUnit4ClassRunner.class`，开启了Spring集成测试支持。<sup>[1][]</sup>与此同时，`@ContextConfiguration`指定了如何加载应用程序上下文。此处我们让它加载`AddressBookConfiguration`里配置的Spring应用程序上下文。

In addition to loading the application context, SpringJUnit4ClassRunner also makes it possible to inject beans from the application context into the test itself via autowiring. Because this test is targeting an AddressService bean, it is autowired into the test. Finally, the testService() method makes calls to the address service and verifies the results.  
除了加载应用程序上下文，`SpringJUnit4ClassRunner`还能通过自动织入从应用程序上下文里向测试本身注入Bean。因为这是一个针对`AddressService` Bean的测试，所以需要将它注入到测试里。最后，`testService()`方法调用地址服务并验证了结果。

Although @ContextConfiguration does a great job of loading the Spring application context, it doesn’t load it with the full Spring Boot treatment. Spring Boot applications are ultimately loaded by SpringApplication, either explicitly (as in listing 2.1) or using SpringBootServletInitializer (which we’ll look at in chapter 8). SpringApplication not only loads the application context, but also enables logging, the loading of external properties (application.properties or application.yml), and other features of Spring Boot. If you’re using @ContextConfiguration, you won’t get those features.  
虽然`@ContextConfiguration`在加载Spring应用程序上下文的过程中做了很多事情，但它并没能加载完整的Spring Boot。Spring Boot应用程序最终是由`SpringApplication`加载的，可以是显式的（像代码2.1那样），也可以使用`SpringBootServletInitializer`（我们会在第8章里看到）。`SpringApplication`不仅会加载应用程序上下文，还会开启日志、加载外部属性（application.properties或application.yml），以及其他Spring Boot特性。如果你用的是`@ContextConfiguration`，就得不到这些特性。

To get those features back in your integration tests, you can swap out @ContextConfiguration for Spring Boot’s @SpringApplicationConfiguration:  
要在集成测试里获得这些特性，可以把`@ContextConfiguration`替换为Spring Boot的`@SpringApplicationConfiguration`：

```
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(
      classes=AddressBookConfiguration.class)
public class AddressServiceTests {
  ...
}
```

The use of @SpringApplicationConfiguration is largely identical to @ContextConfiguration. But unlike @ContextConfiguration, @SpringApplicationConfiguration loads the Spring application context using SpringApplication the same way and with the same treatment it would get if it was being loaded in a production application. This includes the loading of external properties and Spring Boot logging.  
`@SpringApplicationConfiguration`的用法大体上和`@ContextConfiguration`是一样的，但也有不同的地方，`@SpringApplicationConfiguration`使用和`SpringApplication`相同的方式来加载Spring应用程序上下文，就和在生产应用程序里的方式一样。这包括了加载外部属性和Spring Boot日志。

[1]: # "As of Spring 4.2, you can optionally use SpringClassRule and SpringMethodRule as JUnit rule-based alternatives to SpringJUnit4ClassRunner. 在Spring 4.2里，你可以选择基于规则的`SpringClassRule`和`SpringMethodRule`来代替`SpringJUnit4ClassRunner`。"

Suffice it to say that, for the most part, @SpringApplicationConfiguration replaces @ContextConfiguration when writing tests for Spring Boot applications. We’ll certainly use @SpringApplicationConfiguration throughout this chapter as we write tests for our Spring Boot application, including tests that target the web front end of the application.  
有充分的理由可以说，在大多数情况下，为Spring Boot应用程序编写测试时应该用`@SpringApplicationConfiguration`代替`@ContextConfiguration`。在本章中，我们当然也会用`@SpringApplicationConfiguration`来为我们的Spring Boot应用程序编写测试，包括那些面向前端的应用程序。

Speaking of web testing, that’s what we’re going to do next.  
说到Web测试，正式我们接下来要做的。

## 4.2 Testing web applications
## 4.2 测试Web应用程序

One of the nice things about Spring MVC is that it promotes a programming model around plain old Java objects (POJOs) that are annotated to declare how they should process web requests. This programming model is not only simple, it enables you to treat controllers just as you would any other component in your application. You might even be tempted to write tests against your controller that test them as POJOs.  
Spring MVC有一点好处，它的编程模型是围绕POJO（plain old Java object）展开的，在POJO上添加注解声明如何处理Web请求。这种编程模型不仅简单，还让你能像对待应用程序中的其他组件一样来对待这些控制器。你还可以针对这些控制器编写测试，就像测试POJO一样。

For instance, consider the addToReadingList() method from ReadingListController:  
举例来说，考虑`ReadingListController`里的`addToReadingList()`方法：

```
@RequestMapping(method=RequestMethod.POST)
public String addToReadingList(Book book) {
  book.setReader(reader);
  readingListRepository.save(book);
  return "redirect:/readingList";
}
```

If you were to disregard the @RequestMapping method, you’d be left with a rather basic Java method. It wouldn’t take much to imagine a test that provides a mock implementation of ReadingListRepository, calls addToReadingList() directly, and asserts the return value and verifies the call to the repository’s save() method.  
如果忽略`@RequestMapping`注解，你得到的就是一个相当基础的Java方法。你立马就能想到这样一个测试，提供一个`ReadingListRepository`的模拟实现，直接调用`addToReadingList()`，判断返回值并验证对`ReadingListRepository`的`save()`方法有过调用。

The problem with such a test is that it only tests the method itself. While that’s better than no test at all, it fails to test that the method handles a POST request to /readingList. It also fails to test that form fields are properly bound to the Book parameter. And although you could assert that the returned String contains a certain value, it would be impossible to test definitively that the request is, in fact, redirected to /readingList after the method is finished.  
该测试的问题是它仅仅测试了方法本身，当然，这要比没有测试好一点。它没有测试该方法要处理/readingList的POST请求，也没有测试表单域绑定到`Book`参数。虽然你可以判断返回的`String`包含特定值，但没法明确测试请求在方法处理完之后被重定向到/readingList。

To properly test a web application, you need a way to throw actual HTTP requests at it and assert that it processes those requests correctly. Fortunately, there are two options available to Spring Boot application developers that make those kinds of tests possible:  
要恰当地测试一个Web应用程序，你需要扔些实际的HTTP请求给它，确认它能正确地处理那些请求。幸运的是，Spring Boot开发者有两个可选的方案能实现这类测试：

* Spring Mock MVC—Enables controllers to be tested in a mocked approximation of a servlet container without actually starting an application server
* Web integration tests—Actually starts the application in an embedded servlet container (such as Tomcat or Jetty), enabling tests that exercise the application in a real application server
* Spring Mock MVC——能在一个近似真是的模拟Servlet容器里测试控制器，而实际并不用启动一个应用服务器
* Web集成测试——在嵌入式Servlet容器（比如Tomcat或Jetty）里启动应用程序，在真正的应用服务器里执行测试

Each of these kinds of tests has its share of pros and cons. Obviously, starting a server will result in a slower test than mocking a servlet container. But there’s no doubt that server-based tests are closer to the real-world environment that they’ll be running in when deployed to production.  
这两种方法各有利弊。很明显，启动一个应用服务器会比模拟Servlet容器要慢一些，但毫无疑问基于服务器的测试会更接近真实环境，更接近部署到生产环境运行的情况。

We’re going to start by looking at how you can test a web application using Spring’s Mock MVC test framework. Then, in section 4.3, you’ll see how to write tests against an application that’s actually running in an application server.  
接下来，你会看到如何使用Spring Mock MVC测试框架来测试Web应用程序。然后，在4.3节里你会看到如何针对运行在应用服务器里的应用程序编写测试。
