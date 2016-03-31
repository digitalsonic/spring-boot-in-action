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

### 4.2.1 Mocking Spring MVC
### 4.2.1 模拟Spring MVC

Since Spring 3.2, the Spring Framework has had a very useful facility for testing web applications by mocking Spring MVC. This makes it possible to perform HTTP requests against a controller without running the controller within an actual servlet container. Instead, Spring’s Mock MVC framework mocks enough of Spring MVC to make it almost as though the application is running within a servlet container … but it’s not.  
自Spring 3.2开始，Spring Framework就有一套非常有用的Web应用程序测试工具，它就能用来模拟Spring MVC，能在不需要真实Servlet容器的情况下对控制器发送HTTP请求。Spring的Mock MVC框架模拟了很多Spring MVC的功能，让它几乎能和运行在Servlet容器里的应用程序一样，而实际它并没跑在容器里。

To set up a Mock MVC in your test, you can use MockMvcBuilders. This class offers two static methods:  
要在你的测试里设置Mock MVC，可以使用`MockMvcBuilders`，该类提供了两个静态方法：

* standaloneSetup()—Builds a Mock MVC to serve one or more manually created and configured controllers
* webAppContextSetup()—Builds a Mock MVC using a Spring application context, which presumably includes one or more configured controllers
* standaloneSetup()——构建一个Mock MVC，提供一个或多个手工创建并配置的控制器
* webAppContextSetup()——使用Spring应用程序上下文来构建Mock MVC，该上下文里可以包含一个或多个配置好的控制器

The primary difference between these two options is that standaloneSetup() expects you to manually instantiate and inject the controllers you want to test, whereas webAppContextSetup() works from an instance of WebApplicationContext, which itself was probably loaded by Spring. The former is slightly more akin to a unit test in that you’ll likely only use it for very focused tests around a single controller. The latter, however, lets Spring load your controllers as well as their dependencies for a fullblown integration test.  
两者的主要区别在于`standaloneSetup()`希望你手工初始化并注入你要测试的控制器，而`webAppContextSetup()`则基于一个`WebApplicationContext`的实例，通常它是由Spring加载的。前者同单元测试更加接近，你可能只想让它聚焦于单一控制器的测试。而后者让Spring来加载控制器及其依赖，以便进行集成测试。

For our purposes, we’re going to use webAppContextSetup() so that we can test the ReadingListController as it has been instantiated and injected from the application context that Spring Boot has auto-configured.  
对我们而言，我们要用`webAppContextSetup()`，Spring完成了`ReadingListController`的初始化并从Spring Boot自动配置的应用程序上下文里将其注入进来，我们直接对其进行测试。

The webAppContextSetup() takes a WebApplicationContext as an argument. Therefore, we’ll need to annotate the test class with @WebAppConfiguration and use
@Autowired to inject the WebApplicationContext into the test as an instance variable. The following listing shows the starting point for our Mock MVC tests.  
`webAppContextSetup()`接受一个`WebApplicationContext`参数。因此，我们需要为测试类加上'@WebAppConfiguration'注解，使用`@Autowired`将`WebApplicationContext`作为实例变量注入测试类。以下代码演示了Mock MVC测试的执行入口。

__Listing 4.2 Creating a Mock MVC for integration testing controllers__  
__代码4.2 为集成测试控制器创建Mock MVC__

```
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(
             classes = ReadingListApplication.class)
@WebAppConfiguration
public class MockMvcWebTests {
  @Autowired
  private WebApplicationContext webContext;

  private MockMvc mockMvc;

  @Before
  public void setupMockMvc() {
    mockMvc = MockMvcBuilders
            .webAppContextSetup(webContext)
            .build();
  }
}
```

Enables web context testing  
开启Web上下文测试

Injects WebApplicationContext  
注入`WebApplicationContext`

Sets up MockMvc  
设置`MockMvc`

The @WebAppConfiguration annotation declares that the application context created by SpringJUnit4ClassRunner should be a WebApplicationContext (as opposed to a
basic non-web ApplicationContext).  
`@WebAppConfiguration`注解声明了由`SpringJUnit4ClassRunner`创建的应用程序上下文应该是一个`WebApplicationContext`（相对于基本的非Web的`ApplicationContext`）。

The setupMockMvc() method is annotated with JUnit’s @Before, indicating that it should be executed before any test methods. It passes the injected WebApplicationContext into the webAppContextSetup() method and then calls build() to produce a MockMvc instance, which is assigned to an instance variable for test methods to use.  
`setupMockMvc()`方法上添加了JUnit的`@Before`注解，表明它应该在测试方法之前执行。它将`WebApplicationContext`注入`webAppContextSetup()`方法，然后调用`build()`产生了一个`MockMvc`实例，该实例被赋给了一个实例变量以供测试方法使用。

Now that we have a MockMvc, we’re ready to write some test methods. Let’s start with a simple test method that performs an HTTP GET request against /readingList and asserts that the model and view meet our expectations. The following homePage() test method does what we need:  
现在我们有了一个`MockMvc`，已经可以开始写测试方法了。让我们先写个简单的测试方法，向/readingList发送一个HTTP GET请求，判断模型和视图是否满足我们的期望。下面的`homePage()`测试方法就是我们所需要的：

```
@Test
public void homePage() throws Exception {
  mockMvc.perform(MockMvcRequestBuilders.get("/readingList"))
          .andExpect(MockMvcResultMatchers.status().isOk())
          .andExpect(MockMvcResultMatchers.view().name("readingList"))
          .andExpect(MockMvcResultMatchers.model().attributeExists("books"))
          .andExpect(MockMvcResultMatchers.model().attribute("books",
                   Matchers.is(Matchers.empty())));
}
```

As you can see, a lot of static methods are being used in this test method, including static methods from Spring’s MockMvcRequestBuilders and MockMvcResultMatchers, as well as from the Hamcrest library’s Matchers. Before we dive into the details of this test method, let’s add a few static imports so that the code is easier to read:  
如你所见，在这个测试方法里我们使用了很多静态方法，包括Spring的`MockMvcRequestBuilders`和`MockMvcResultMatchers`里的静态方法，还有Hamcrest库的`Matchers`里的静态方法。在我们深入探讨这个测试方法前，先加点静态`import`，这样代码看起来更清爽一些：

```
import static org.hamcrest.Matchers.*;
import static org.springframework.test.web.servlet.request.
        ➥ MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.
        ➥ MockMvcResultMatchers.*;
```

With those static imports in place, the test method can be rewritten like this:  
有了这些静态`import`后，测试方法可以稍作调整：

```
@Test
public void homePage() throws Exception {
mockMvc.perform(get("/readingList"))
        .andExpect(status().isOk())
        .andExpect(view().name("readingList"))
        .andExpect(model().attributeExists("books"))
        .andExpect(model().attribute("books", is(empty())));
}
```

Now the test method almost reads naturally. First it performs a GET request against /readingList. Then it expects that the request is successful (isOk() asserts an HTTP 200 response code) and that the view has a logical name of readingList. It also asserts that the model contains an attribute named books, but that attribute is an empty collection. It’s all very straightforward.  
现在这个测试方法读起来就很自然了。首先向/readingList发起一个GET请求，然后希望该请求处理成功（`isOk()`会判断HTTP 200响应码），并且视图的逻辑名称是`readingList`，测试里还要判断模型包含一个名为`books`的属性，该属性是一个空集合。所有的断言都很直观。

The main thing to note here is that at no time is the application deployed to a web server. Instead it’s run within a mocked out Spring MVC, just capable enough to handle the HTTP requests we throw at it via the MockMvc instance.  
值得一提的是此处完全不需要将应用程序部署到Web服务器上，它是运行在模拟的Spring MVC中的，刚好能通过`MockMvc`实例来处理我们扔给它的HTTP请求。

Pretty cool, huh?  
太酷了，不是么？

Let’s try one more test method. This time we’ll make it a bit more interesting by actually sending an HTTP POST request to post a new book. We should expect that after the POST request is handled, the request will be redirected back to /readingList and that the books attribute in the model will contain the newly added book. The following listing shows how we can use Spring’s Mock MVC to do this kind of test.  
让我们再来看一个测试方法，这次会更有趣一些，我们实际发送一个HTTP POST请求提交一本新书。我们应该期待在POST请求处理后重定向回/readingList，模型中将包含新添加的图书。以下代码演示了如何通过Spring的Mock MVC来实现这个测试。

__Listing 4.3 Testing the post of a new book__  
__代码4.3 测试提交一本新书__

```
@Test
public void postBook() throws Exception {
  mockMvc.perform(post("/readingList")
         .contentType(MediaType.APPLICATION_FORM_URLENCODED)
         .param("title", "BOOK TITLE")
         .param("author", "BOOK AUTHOR")
         .param("isbn", "1234567890")
         .param("description", "DESCRIPTION"))
         .andExpect(status().is3xxRedirection())
         .andExpect(header().string("Location", "/readingList"));

  Book expectedBook = new Book();
  expectedBook.setId(1L);
  expectedBook.setReader("craig");
  expectedBook.setTitle("BOOK TITLE");
  expectedBook.setAuthor("BOOK AUTHOR");
  expectedBook.setIsbn("1234567890");
  expectedBook.setDescription("DESCRIPTION");

  mockMvc.perform(get("/readingList"))
         .andExpect(status().isOk())
         .andExpect(view().name("readingList"))
         .andExpect(model().attributeExists("books"))
         .andExpect(model().attribute("books", hasSize(1)))
         .andExpect(model().attribute("books",
                     contains(samePropertyValuesAs(expectedBook))));
```

Performs POST request  
执行POST请求

Sets up expected book  
配置期望的图书

Performs GET request  
执行GET请求

Obviously, the test in listing 4.3 is a bit more involved. It’s actually two tests in one method. The first part posts the book and asserts the results from that request. The second part performs a fresh GET request against the home page and asserts that the newly created book is in the model.  
很明显，代码4.3里的测试更复杂一些，实际是两个测试放在一个方法里。第一部分提交图书并检查了请求的结果，第二部分执行了一次对主页的GET请求，检查新建的图书是否在模型中。

When posting the book, we must make sure we set the content type to “application/ x-www-form-urlencoded” (with MediaType.APPLICATION_FORM_URLENCODED) as that will be the content type that a browser will send when the book is posted in the running application. We then use the MockMvcRequestBuilders’s param() method to set the fields that simulate the form being submitted. Once the request has been performed, we assert that the response is a redirect to /readingList.  
在提交图书时，我们必须确保将内容类型设置为“application/x-www-form-urlencoded”（通过`MediaType.APPLICATION_FORM_URLENCODED`），这才是运行应用程序时浏览器会发送的内容类型。随后用`MockMvcRequestBuilders`的`param`方法设置表单域，模拟要提交的表单。一旦请求执行后，我们要检查响应是否是一个到/readingList的重定向。

Assuming that much of the test method passes, we move on to part two. First, we set up a Book object that contains the expected values. We’ll use this to compare with the value that’s in the model after fetching the home page.  
假定以上测试都通过，我们进入第二部分。首先设置一个`Book`对象，包含想要的值。我们将用这个对象来和首页获取的模型的值进行对比。

Then we perform a GET request for /readingList. For the most part, this is no different than how we tested the home page before, except that instead of an empty collection in the model, we’re checking that it has one item, and that the item is the same as the expected Book we created. If so, then our controller seems to be doing its job of saving a book when one is posted to it.  
随后对/readingList发起一个GET请求，大部分内容和我们之前测试主页时一样，除了之前模型中的那个空集合，现在有了一个集合项，它的内容和我们创建的`expectedBook`一致。如此一来，我们的控制器看起来完成了保存发送给它的图书的工作。

So far, these tests have assumed an unsecured application, much like the one we wrote in chapter 2. But what if we want to test a secured application, such as the one from chapter 3?  
至此，这些测试完成了对一个未经保护的应用程序的验证，就和我们在第2章里写的应用程序很类似。但如果我们想要测试一个安全加固过的应用程序呢，比如我们在第3章里写的那个，该怎么办？

### 4.2.2 Testing web security
### 4.2.2 测试Web安全

Spring Security offers support for testing secured web applications easily. In order to take advantage of it, you must add Spring Security’s test module to your build. The following testCompile dependency in Gradle is all you need:  
Spring Security让你能非常方便地测试安全加固后的Web应用程序，为了利用这点优势，你必须在项目里添加Spring Security的测试模块。在Gradle里，以下`testCompile`依赖就是你所需要的：

```
testCompile("org.springframework.security:spring-security-test")
```

Or if you’re using Maven, add the following <dependency> to your build:  
或者，如果你用的是Maven，添加以下`<dependency>`：

```
<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-test</artifactId>
  <scope>test</scope>
</dependency>
```

With Spring Security’s test module in your application’s classpath, you just need to apply the Spring Security configurer when creating the MockMvc instance:  
应用程序的Classpath里有了Spring Security的测试模块之后，只需在创建`MockMvc`实例时运用Spring Security的配置器就好了：

```
@Before
public void setupMockMvc() {
  mockMvc = MockMvcBuilders
    .webAppContextSetup(webContext)
    .apply(springSecurity())
    .build();
}
```

The springSecurity() method returns a Mock MVC configurer that enables Spring Security for Mock MVC. By simply applying it as shown here, Spring Security will be in play on all requests performed through MockMvc. The specific security configuration will depend on how you’ve configured Spring Security (or how Spring Boot has auto-configured Spring Security). In the case of the reading-list application, it’s the same security configuration we created in SecurityConfig.java in chapter 3.  
`springSecurity()`方法返回了一个Mock MVC配置器，为Mock MVC开启了Spring Security支持。只需像上面这样运用就行了，Spring Security会介入`MockMvc`上执行的每个请求。具体的安全配置取决于你是如何配置Spring Security的（或者Spring Boot是如何自动配置Spring Security的）。在阅读列表这个应用程序里，这个配置与我们在第3章里创建的`SecurityConfig.java`里的配置是一样的。

> THE SPRINGSECURITY() METHOD springSecurity() is a static method of SecurityMockMvcConfigurers, which I’ve statically imported for readability’s sake.  
__springSecurity()方法__ `springSecurity()`是`SecurityMockMvcConfigurers`的一个静态方法，出于可读性考虑我已经将其静态导入了。

With Spring Security enabled, we can no longer simply request the home page and expect an HTTP 200 response. If the request isn’t authenticated, we should expect a redirect to the login page:  
开启了Spring Security之后，我们不能再简单地请求主页并期待HTTP 200响应了。如果请求未经身份验证，我们应该期待重定向到登录页面：

```
@Test
public void homePage_unauthenticatedUser() throws Exception {
  mockMvc.perform(get("/"))
    .andExpect(status().is3xxRedirection())
    .andExpect(header().string("Location",
                               "http://localhost/login"));
}
```

But how can we perform an authenticated request? Spring Security offers two annotations that can help:  
但是我们又该如何发起一个经过身份验证的请求呢？Spring Security提供了两个注解：

* @WithMockUser—Loads the security context with a UserDetails using the given username, password, and authorization
* @WithUserDetails—Loads the security context by looking up a UserDetails object for the given username
* `@WithMockUser`——加载安全上下文，其中包含一个`UserDetails`，使用了给定的用户名、密码和授权
* `@WithUserDetails`——根据给定的用户名查找`UserDetails对象`，加载安全上下文

In both cases, Spring Security’s security context is loaded with a UserDetails object that is to be used for the duration of the annotated test method. The @WithMockUser annotation is the most basic of the two. It allows you to explicitly declare a UserDetails to be loaded into the security context:  
在这两种情况下，Spring Security的安全上下文都会加载一个`UserDetails`对象，在添加了该注解的测试方法运行过程中都会使用该对象。`@WithMockUser`注解是两者里比较基础的那个，它允许你显式声明一个`UserDetails`并加载到安全上下文里：

```
@Test
@WithMockUser(username="craig",
              password="password",
              roles="READER")
public void homePage_authenticatedUser() throws Exception {
  ...
}
```

As you can see, @WithMockUser bypasses the normal lookup of a UserDetails object and instead creates one with the values specified. For simple tests, this may be fine. But for our test, we need a Reader (which implements UserDetails) instead of the generic UserDetails that @WithMockUser creates. For that, we’ll need @WithUserDetails.  
如你所见，`@WithMockUser`绕过了对`UserDetails`对象的正常查询，用给定的值创建了一个`UserDetails`对象取而代之。在简单的测视里，这就够用了。但对我们的测试而言，我们需要一个`Reader`（实现了`UserDetails`）而非`@WithMockUser`创建的通用`UserDetails`。为此，我们需要`@WithUserDetails`。

The @WithUserDetails annotation uses the configured UserDetailsService to load the UserDetails object. As you’ll recall from chapter 3, we configured a UserDetailsService bean that looks up and returns a Reader object for a given username. That’s perfect! So we’ll annotate our test method with @WithUserDetails, as shown in the following listing.  
`@WithUserDetails`注解使用事先配置好的`UserDetailsService`来加载`UserDetails`对象。回想一下第3章，我们配置了一个`UserDetailsService` Bean，它会根据给定的用户名查找并范围一个`Reader`对象。太完美了！所以我们要为测试方法添加`@WithUserDetails`注解，就像下面的代码一样。

__Listing 4.4 Testing a secured method with user authentication__  
__代码4.4 测试带有用户身份验证的安全加固方法__

```
@Test
@WithUserDetails("craig")
public void homePage_authenticatedUser() throws Exception {
  Reader expectedReader = new Reader();
  expectedReader.setUsername("craig");
  expectedReader.setPassword("password");
  expectedReader.setFullname("Craig Walls");

  mockMvc.perform(get("/"))
      .andExpect(status().isOk())
      .andExpect(view().name("readingList"))
      .andExpect(model().attribute("reader",
                         samePropertyValuesAs(expectedReader)))
      .andExpect(model().attribute("books", hasSize(0)));

}
```

Uses “craig” user  
使用“craig”用户

Sets up expected Reader  
配置期望的`Reader`

Performs GET request  
发起GET请求

In listing 4.4, we use @WithUserDetails to declare that the “craig” user should be loaded into the security context for the duration of this test method. Knowing that the Reader will be placed into the model, the method starts by creating an expected Reader object that it can compare with the model later in the test. Then it performs the GET request and asserts the view name and model contents, including the model attribute with the name “reader”.  
在代码4.4里，我们通过`@WithUserDetails`注解声明了要在测试方法执行过程中向安全上下文里加载“craig”用户。`Reader`会被放入模型之中，该测试方法先创建了一个期望的`Reader`对象，后续可以用来进行比较。随后发起GET请求，对视图名和模型内容做了断言，包括名为“reader”的模型属性。

Once again, no servlet container is started up to run these tests. Spring’s Mock MVC takes the place of an actual servlet container. The benefit of this approach is that the test methods run faster because they don’t have to wait for the server to start. Moreover, there’s no need to fire up a web browser to post the form, so the test is simpler and faster.  
同样的，此处没有启动Servlet容器来运行这些测试，Spring的Mock MVC取代了实际的Servlet容器。这样做的好处是测试方法运行相对较快，因为不需要等待服务器启动。而且，不需要打开Web浏览器发送表单，因此测试比较简单快速。

On the other hand, it’s not a complete test. It’s better than simply calling the controller methods directly, but it doesn’t truly exercise the application in a web browser and verify the rendered view. To do that, we’ll need to start a real web server and hit it with a real web browser. Let’s see how Spring Boot can help us start a real web server for our tests.  
另一方面，这并不是一个完整的测试。它比直接简单地调用控制器方法要好点，但它并没有真的在Web浏览器里执行应用程序，验证呈现出的视图。为此，我们需要启动一个真正的Web服务器，用真实浏览器来访问它。让我们来看看Spring Boot是如何帮我们启动一个真实的Web服务器来帮助测试的。

## 4.3 Testing a running application
## 4.3 测试运行中的应用程序

When it comes to testing web applications, nothing beats the real thing. Firing up the application in a real server and hitting it with a real web browser is far more indicative of how it will behave in the hands of users than poking at it with a mock testing engine.  
在测试Web应用程序时，我们都还没接触到实质的内容。在真实的服务器里启动应用程序，用真实的Web浏览器访问它，这样比使用模拟的测试引擎更能表现应用程序在用户端的行为。

But real tests in real servers with real web browsers can be tricky. Although there are build-time plugins for deploying applications in Tomcat or Jetty, they are clunky to set up. Moreover, it’s nearly impossible to run any one of a suite of many such tests in isolation or without starting up your build tool.  
但是用真实的Web浏览器在真实的服务器上运行测试会很麻烦。虽然有构建时的插件能把应用程序部署到Tomcat或者Jetty里，但它们配置起来都很不便。而且几乎不可能隔离运行这么多测试，或者不启动构建工具。

Spring Boot, however, has a solution. Because Spring Boot already supports running embedded servlet containers such as Tomcat or Jetty as part of the running application, it stands to reason that the same mechanism could be used to start up the application along with its embedded servlet container for the duration of a test.  
然而Spring Boot找到了解决方案。因为它支持将Tomcat或Jetty这样的嵌入式Servlet容器作为运行中的应用程序的一部分，可以运用相同的机制，在测试过程中用嵌入式Servlet容器来启动应用程序。

That’s exactly what Spring Boot’s @WebIntegrationTest annotation does. By annotating a test class with @WebIntegrationTest, you declare that you want Spring Boot to not only create an application context for your test, but also to start an embedded servlet container. Once the application is running along with the embedded container, you can issue real HTTP requests against it and make assertions against the results.  
Spring Boot的`@WebIntegrationTest`注解就是这么做的。在测试类上添加`@WebIntegrationTest`注解，以此声明你不仅希望Spring Boot为测试创建应用程序上下文，还要启动过一个嵌入式的Servlet容器。一旦应用程序运行在嵌入式容器里之后，你就可以发起真实的HTTP请求，对结果做断言了。

For example, consider the simple web test in listing 4.5, which uses @WebIntegrationTest to start the application along with a server and uses Spring’s RestTemplate to perform HTTP requests against the application.  
举例来说，考虑代码4.5里的那段简单的Web测试，它用了`@WebIntegrationTest`，在服务器里启动了应用程序，使用Spring的`RestTemplate`对应用程序发起HTTP请求。

__Listing 4.5 Testing a web application in-server__  
__代码4.5 测试运行在服务器里的Web应用程序__

```
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(
      classes=ReadingListApplication.class)
@WebIntegrationTest
public class SimpleWebTest {

  @Test(expected=HttpClientErrorException.class)
  public void pageNotFound() {
    try {
      RestTemplate rest = new RestTemplate();
      rest.getForObject(
           "http://localhost:8080/bogusPage", String.class);
      fail("Should result in HTTP 404");
    } catch (HttpClientErrorException e) {
      assertEquals(HttpStatus.NOT_FOUND, e.getStatusCode());
      throw e;
    }
  }

}
```

Runs test in server  
在服务器里运行测试

Performs GET request  
发起GET请求

Asserts HTTP 404 (not found) response  
判断HTTP 404 (NOT FOUND)响应

Although this is a very simple test, it sufficiently demonstrates how to use the @WebIntegrationTest to start the application with a server. The actual server that’s started will be determined in the same way it would be if we were running the application at the command line. By default, it starts Tomcat listening on port 8080. Optionally, however, it could start Jetty or Undertow if either of those is in the classpath.  
虽然这个测试非常简单，但足以演示如何使用`@WebIntegrationTest`在服务器里启动应用程序了。实际启动的服务器究竟是哪个，这个判断逻辑与在命令行里运行应用程序时一样的。默认情况下，会启动一个监听8080端口的Tomcat。但是，如果Classpath里有Jetty或者Undertow，它也能启动这些服务器。

The body of the test method is written assuming that the application is running and listening on port 8080. It uses Spring’s RestTemplate to make a request for a nonexistent page and asserts that the response from the server is an HTTP 404 (not found). The test will fail if any other response is returned.  
测试方法的主体部分假设应用程序已经运行，监听了8080端口。它使用了Spring的`RestTemplate`对一个不存在的页面发起请求，判断服务器的响应是否是HTTP 404 (NOT FOUND)。如果返回了其他响应测试即告失败。

### 4.3.1 Starting the server on a random port
### 4.3.1 用随机端口启动服务器

As mentioned, the default behavior is to start the server listening on port 8080. That’s fine for running a single test at a time on a machine where no other server is already listening on port 8080. But if you’re like me, you’ve probably always got something listening on port 8080 on your local machine. In that case, the test would fail because the server wouldn’t start due to the port collision. There must be a better way.  
前面提到过，此处的默认行为是启动服务器监听8080端口。在一台机器上一次只运行一个测试的话，这没什么问题，因为没有其他服务器监听8080端口。但如果你和我一样，在本机总是有其他服务器在监听8080端口该怎么办？这时测试就会失败，因为端口冲突，服务器启动不了。一定有个更好的办法。

Fortunately, it’s easy enough to ask Spring Boot to start up the server on a randomly selected port. One way is to set the server.port property to 0 to ask Spring Boot to select a random available port. @WebIntegrationTest accepts an array of String for its value attribute. Each entry in the array is expected to be a name/value pair, in the form name=value, to set properties for use in the test. To set server.port you can use @WebIntegrationTest like this:  
幸运的是，可以很方便地让Spring Boot在随机选择的端口上启动服务器。一种办法是将`server.port`属性设置为0，让Spring Boot选择一个随机的可用端口。`@WebIntegrationTest`的`value`属性接受一个`String`数组，数组中的每项都是一个键值对，形如`name=value`，用来设置测试中使用的属性。要设置`server.port`，你可以这样做：

```
@WebIntegrationTest(value={"server.port=0"})
```

Or, because there’s only one property being set, it can take a simpler form:  
或者，因为只要设置一个属性，还能有个更简单的形式：

```
@WebIntegrationTest("server.port=0")
```

Setting properties via the value attribute is handy in the general sense, but @WebIntegrationTest also offers a randomPort attribute for a more expressive way of asking the server to be started on a random port. You can ask for a random port by setting randomPort to true:  
通过`value`属性来设置属性通常还算方便，但`@WebIntegrationTest`还提供了一个`randomPort`属性，更明确地表示让服务器以随机端口启动。你可以将`randomPort`设置为`true`，启用随机端口：

```
@WebIntegrationTest(randomPort=true)
```

Now that we have the server starting on a random port, we need to be sure we use the correct port when making web requests. At the moment, the getForObject() method is hard-coded with port 8080 in its URL. If the port is randomly chosen, how can we construct the request to use the right port?  
既然我们在随机端口上启动了服务器，就需要在发起Web请求时确保使用正确的端口。此时的`getForObject()`方法在URL里硬编码了8080端口。如果端口是随机选择的，那在构造请求时又该怎么确定正确的端口呢？

First we’ll need to inject the chosen port as an instance variable. To make this convenient, Spring Boot sets a property with the name local.server.port to the value of the chosen port. All we need to do is use Spring’s @Value to inject that property:  
首先，我们需要以实例变量的形式注入选中的端口。为了方便起见，Spring Boot将`local.server.port`的值设置为了选中的端口。我们只需使用Spring的`@Value`注解将其注入即可：

```
@Value("${local.server.port}")
private int port;
```

Now that we have the port, we just need to make a slight change to the getForObject() call to use it:  
有了端口之后，只需对`getForObject()`稍作修改，使用这个`port`就好了：

```
rest.getForObject(
    "http://localhost:{port}/bogusPage", String.class, port);
```

Here we’ve traded the hardcoded 8080 for a {port} placeholder in the URL. By passing the port property as the last parameter in the getForObject() call, we can be assured that the placeholder will be replaced with whatever value was injected into port.  
这里我们在URL里把硬编码的8080改为`{port}`占位符。在`getForObject()`调用里把`port`属性作为最后一个参数传入，就能确保该占位符被替换为注入到`port`里的值了。

### 4.3.2 Testing HTML pages with Selenium
### 4.3.2 使用Selenium测试HTML页面

RestTemplate is fine for simple requests and it’s perfect for testing REST endpoints. But even though it can be used to make requests against URLs that return HTML pages, it’s not very convenient for asserting the contents of the page or performing operations on the page itself. At best, you’ll be able to assert the precise content of the resulting HTML (which will result in fragile tests). But you won’t easily be able to assert selected content on the page or perform operations such as clicking links or submitting forms.  
`RestTemplate`对于简单的请求而言很好用，也是测试REST端点的理想工具。但是，就算它能对返回HTML页面的URL发起请求，也不方便对页面内容或者页面上执行的操作进行断言。最好可以精确判断结果HTML里的内容（这种测试很脆弱）。不过你无法方便地判断页面上选中的内容，或者执行诸如点击链接或提交表单这样的操作。

A better choice for testing HTML applications is Selenium (www.seleniumhq.org). Selenium does more than just perform requests and fetch the results for you to verify. Selenium actually fires up a web browser and executes your test within the context of the browser. It’s as close as you can possibly get to performing the tests manually with your own hands. But unlike manual testing, Selenium tests are automated and repeatable.  
测试HTML应用程序有一个更好的选择——Selenium（[www.seleniumhq.org](http://www.seleniumhq.org)），它的功能远不止为你提交请求并获取结果，它能实际打开一个Web浏览器，在浏览器的上下文中执行你的测试。Selenium能尽可能地接近手动执行测试，但与手工测试不同，Selenium的测试是自动的，而且可以重复运行。

To test our reading list application using Selenium, let’s write a test that fetches the home page, fills out the form for a new book, posts the form, and then finally asserts that the landing page includes the newly added book.  
为了用Selenium测试我们的阅读列表应用程序，让我们先一个测试来获取首页，为新书填写表单，提交表单，随后判断返回的页面里是否包含新添加的图书。

First we’ll need to add Selenium to the build as a test dependency:  
首先需要把Selenium作为测试依赖添加到项目里：

```
testCompile("org.seleniumhq.selenium:selenium-java:2.45.0")
```

Now we can write the test class. The following listing shows a basic template for a Selenium test that uses Spring Boot’s @WebIntegrationTest.  
现在就可以编写测试了，以下代码是一个基本的Selenium测试模板，使用了Spring Boot的`@WebIntegrationTest`。

__Listing 4.6 A template for Selenium testing with Spring Boot__  
__代码4.6 在Spring Boot里使用Selenium测试的模板__

```
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(
      classes=ReadingListApplication.class)
@WebIntegrationTest(randomPort=true)
public class ServerWebTests {

  private static FirefoxDriver browser;

  @Value("${local.server.port}")
  private int port;

  @BeforeClass
  public static void openBrowser() {
    browser = new FirefoxDriver();
    browser.manage().timeouts()
        .implicitlyWait(10, TimeUnit.SECONDS);
  }

  @AfterClass
  public static void closeBrowser() {
    browser.quit();
  }

}
```

Starts on a random port  
用随机端口启动

Injects the port  
注入端口号

Sets up Firefox driver  
配置Firefox驱动

Shuts down browser  
关闭浏览器

As with the simpler web test we wrote earlier, this class is annotated with @WebIntegrationTest and sets randomPort to true so that the application will be started and run with a server listening on a random port. And, as before, that port is injected into the port property so that we can use it to construct URLs to the running application.  
和我们之前写的更简单的Web测试一样，这个类添加了`@WebIntegrationTest`注解，将`randomPort`设置为`true`，这样应用程序启动后会运行一个监听随机端口的服务器。同样的，端口号被注入到`port`属性里，这样我们就能用它来构造指向运行中的应用程序的URL了。

The static openBrowser() method creates a new instance of FirefoxDriver, which will open a Firefox browser (it will need to be installed on the machine running the test). When we write our test method, we’ll perform browser operations through the FirefoxDriver instance. The FirefoxDriver is also configured to wait up to 10 seconds when looking for any elements on the page (in case those elements are slow to load).  
静态方法`openBrowser()`会创建一个`FirefoxDriver`的实例，它将打开一个Firefox浏览器（需要在运行测试的服务器上安装该浏览器）。我们的测试方法将通过`FirefoxDriver`实例来执行浏览器操作。在页面上查找元素时，`FirefoxDriver`配置了10秒的等候时间（以防那些元素加载很慢）。

After the test has completed, we’ll need to shut down the Firefox browser. Therefore, closeBrowser() calls quit() on the FirefoxDriver instance to bring it down.  
测试执行完毕后，我们需要关闭Firefox浏览器，因此在`closeBrowser()`里要调用`FirefoxDriver`实例的`quit()`方法，关闭浏览器。

>PICK YOUR BROWSER Although we’re testing with Firefox, Selenium also provides drivers for several other browsers, including Internet Explorer, Google’s Chrome, and Apple’s Safari. Not only can you use other browsers, it’s probably a good idea to write your tests to use any and all browsers you want to support.  
__选择浏览器__ 虽然我们用Firefox进行了测试，但Selenium还提供了不少其他浏览器的驱动，包括Internet Explorer、Google的Chrome，还有Apple的Safari。不仅可以使用其他浏览器，要是你的测试能使用各种你想支持的浏览器进行测试，也许也是个不错的想法。

Now we can write our test method. As a reminder, we want to load the home page, fill in and submit the form, and then assert that we land on a page that includes our newly added book in the list. The following listing shows how to do this with Selenium.  
现在可以开始编写我们的测试方法了，给你提个醒，我们想要加载首页，填充并发送表单，然后判断我们登录到的页面上包含刚刚新添加的图书。下面的代码演示了如何用Selenium实现这个功能。

__Listing 4.7 Testing the reading-list application with Selenium__  
__代码4.7 用Selenium测试阅读列表应用程序__

```
@Test
public void addBookToEmptyList() {
  String baseUrl = "http://localhost:" + port;

  browser.get(baseUrl);

  assertEquals("You have no books in your book list",
               browser.findElementByTagName("div").getText());

  browser.findElementByName("title")
  .sendKeys("BOOK TITLE");
  browser.findElementByName("author")
         .sendKeys("BOOK AUTHOR");
  browser.findElementByName("isbn")
         .sendKeys("1234567890");
  browser.findElementByName("description")
         .sendKeys("DESCRIPTION");
  browser.findElementByTagName("form")
         .submit();

  WebElement dl =
      browser.findElementByCssSelector("dt.bookHeadline");
  assertEquals("BOOK TITLE by BOOK AUTHOR (ISBN: 1234567890)",
               dl.getText());
  WebElement dt =
      browser.findElementByCssSelector("dd.bookDescription");
  assertEquals("DESCRIPTION", dt.getText());
}
```

Fetches the home page  
获取主页

Asserts an empty book list  
判断图书列表是否为空

Fills in and submits form  
填充并发送表单

Asserts new book in list  
判断列表中是否包含新书

The very first thing that the test method does is use the FirefoxDriver to perform a GET request for the reading list’s home page. It then looks for a <div> element on the page and asserts that its text indicates that no books are in the list.  
该测试方法做的第一件事是使用`FirefoxDriver`来发起GET请求，获取阅读列表的主页，随后查找页面里的一个`<div>`元素，从它的文本里判断出列表里没有图书。

The next several lines look for the fields in the form and use the driver’s sendKeys() method to simulate keystroke events on those field elements (essentially filling in those fields with the given values). Finally, it looks for the <form> element and submits it.  
接下来的几行是在查找表单里的元素，使用驱动的`sendKeys()`方法模拟敲击键盘事件（实际上就是用给定的值填充那些表单域）。最后，找到`<form>`元素并提交。

After the form submission is processed, the browser should land on a page with the new book in the list. So the final few lines look for the <dt> and <dd> elements in that list and assert that they contain the data that the test submitted in the form.  
提交的表单被处理后，浏览器就会跳到一个页面，上面的列表里包含了新添加的图书。因此最后的几行就是在查找列表里的`<dt>`和`<dd>`元素，判断其中是否包含测试表单里提交的数据。

When you run this test, you’ll see the browser pop up and load the reading-list application. If you pay close attention, you’ll see the form filled out, as if by a ghost. But it’s no spectre using your application—it’s the test.  
运行测试时，你会看到浏览器被打开，加载了阅读列表应用程序。如果你够仔细，还会看到填充表单的过程，就好像幽灵在操作，当然，并没有幽灵在使用你的应用程序——这只是一个测试。

The main thing to notice about this test is that @WebIntegrationTest was able to start up the application and server for us so that Selenium could start poking at it with a web browser. But what’s especially interesting about how this works is that you can use the test facilities of your IDE to run as many or as few of these tests as you want, without having to rely on some plugin in your application’s build to start a server for you.  
这个测试里需要注意的`@WebIntegrationTest`可以为我们启动应用程序和服务器，这样Selenium才可以用Web浏览器执行测试。但真正有趣的是你可以使用IDE的测试功能来运行测试，随便你想跑几次都行，无需依赖构建过程中的某些插件来为你启动服务器。

If testing with Selenium is something that you think you’ll find useful, you should check out Selenium WebDriver in Practice by Yujun Liang and Alex Collins (http://manning.com/liang/), which goes into far more details about testing with Selenium.  
要是你觉得使用Selenium进行测试对你有用，可以阅读Yujun Liang和Alex Collins的《Selenium WebDriver in Practice》（[http://manning.com/liang/](http://manning.com/liang/)），该书更深入地讨论了Selenium测试的细节。

## 4.4 Summary
## 4.4 小结

Testing is an important part of developing quality software. Without a good suite of tests, you’ll never know for sure if your application is doing what it’s expected to do.  
测试是开发高质量软件的重要一环，没有好的测试，你永远都不无法保证你的应用程序能如你期望的那样运作。

For unit tests, which focus on a single component or a method of a component, Spring isn’t really necessary. The benefits and techniques promoted by Spring—loose coupling, dependency injection, and interface-driven design—make writing unit tests easy. But Spring doesn’t need to be directly involved in unit tests.  
对单元测试而言，它专注于单一组件或组件中的一个方法，此处并不一定要用Spring。Spring提供了一些好处和技术——松耦合、依赖注入和接口驱动设计，这些都让编写单元测试变容易了。但Spring不用直接涉足单元测试。

Integration-testing multiple components, however, begs for help from Spring. In fact, if Spring is responsible for wiring those components up at runtime, then Spring should also be responsible for wiring them up in integration tests.  
集成测试会涉及众多组件，这时就要靠Spring来帮忙了。实际上，如果在运行时Spring负责拼装那些组件，那么在集成测试里Spring同样应该肩负这一职责。

The Spring Framework provides integration-testing support in the form of a JUnit class runner that loads a Spring application context and enables beans from the context to be injected into a test. Spring Boot builds upon Spring integration-testing support with a configuration loader that loads the application context in the same way as Spring Boot itself, including support for externalized properties and Spring Boot logging.  
Spring Framework以JUnit类运行器的方式提供了集成测试支持，它会加载Spring应用程序上下文，把上下文里的Bean注入到测试里。Spring Boot在Spring的集成测试之上又增加了配置加载器，以Spring Boot的方式加载应用程序上下文，包括对外置属性的支持和Spring Boot日志。

Spring Boot also enables in-container testing of web applications, making it possible to fire up your application to be served by the same container that it will be served by when running in production. This gives your tests the closest thing to a real-world environment for verifying the behavior of the application.  
Spring Boot还支持容器内测试Web应用程序，让你能用和生产环境一样的容器来启动应用程序。这让你的测试更接近真实运行环境，验证应用程序的行为。

At this point we’ve built a rather complete (albeit simple) application that leverages Spring Boot starters and auto-configuration to handle the grunt work so that we can focus on writing our application. And we’ve also seen how to take advantage of Spring Boot’s support for testing the application. Coming up in the next couple of chapters, we’re going to take a slightly different tangent and explore the ways that Groovy can make developing Spring Boot applications even easier. We’ll start in the next chapter by looking at a few features from the Grails framework that have made their way into Spring Boot.  
此时我们已经构建了一个相当完整的应用程序（虽然有点简单），它利用Spring Boot的起步依赖和自动配置来处理低级的工作，让我们能专心开发自己的应用程序。我们也看到了如何使用Spring Boot的支持来测试应用程序。在后续几章里，我们会看到一些不同的东西，了解到Groovy能让Spring Boot应用程序的开发更加简单。下一章里会先了解一些Grails框架的特性，看看它们是如何进入Spring Boot的。
