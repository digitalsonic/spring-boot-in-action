# 第4章 测试


>本章内容

>* 集成测试
* 在服务器里测试应用程序
* Spring Boot的测试辅助工具

有人说，如果你不知道要去哪，走就是了。但在软件开发领域里，如果你都不知道要去哪里，那结果往往是做出一个满是bug的应用程序，没人能用得了。

在编写应用程序时，搞明白自己在干什么的最佳途径就是写点测试，确定应用程序的行为符合预期。如果测试失败了，你就有活要干了。如果测试通过了，那你就成功了（至少在你觉得还有其他测试要写之前，是这样的）。

究竟是在编写业务代码之前还是之后写测试，这并不重要，重要的是你写测试的目的并不仅仅是验证代码的准确性，还要确认它符合你的预期。测试也是一道保障，确认应用程序在改进的同时不会破坏已有的东西。

在编写单元测试的时候，Spring通常并不需要介入。Spring鼓励松耦合、接口驱动的设计，这些都能让你很轻松地编写单元测试，而且在写单元测试时并不需要用到Spring。

但是，集成测试要用到Spring。如果你的生产应用程序使用Spring来配置并组装组件，那么在你在测试里同样需要让它来配置并组装那些组件。

Spring的`SpringJUnit4ClassRunner`可以帮你在基于JUnit的应用程序测试里加载Spring应用程序上下文。在测试Spring Boot应用程序时，Spring Boot除了拥有Spring的集成测试支持，还开启了自动配置和Web服务器，并提供了不少有用的测试辅助工具。

在本章中，我们会看到Spring Boot的各种集成测试支持。让我们先来看看如何在Spring Boot应用程序上下文里做测试。

## 4.1 集成测试自动配置

在Spring Framework的众多工作之中，最核心的就是将所有组件编织在一起构成一个应用程序。整个过程就是读取配置说明（可以是XML、基于Java的配置、基于Groovy的配置或其他类型的配置），在应用程序上下文里初始化Bean，将Bean注入依赖它们的其他Bean中。

对Spring应用程序进行集成测试时，让Spring用和生产环境一样的方式组装测试目标Bean是非常重要的。当然，你也可以手动初始化组件，并将它们注入其他组件，但对那些大型应用程序来说，这是项费时费力的工作。而且，Spring提供了额外的辅助功能，比如组件扫描、自动织入和声明性切面（比如缓存、事务和安全）。你要把这些活都干了，基本也就是把Spring再造了一次，最好还是让Spring替你把重活都做了吧，哪怕是在集成测试里。

Spring自1.1.1版就对集成测试提供了极佳的支持。自Spring 2.5开始，集成测试支持的形式就变成了`SpringJUnit4ClassRunner`，这是一个JUnit类运行器，会为JUnit测试加载Spring应用程序上下文，并为测试类自动织入所需的Bean。

举例来说，看一下代码清单4-1，这是一个非常基本的Spring集成测试。

__代码清单4-1 用`SpringJUnit4ClassRunner`对Spring应用程序进行集成测试__

>原书P77~78代码

>文字

>Loads application context：加载应用程序上下文

>Injects address service：注入地址服务

>Tests address service：测试地址服务

如你所见，`AddressServiceTests`上加注了`@RunWith`和`@ContextConfiguration`注解。`@RunWith`的参数是`SpringJUnit4ClassRunner.class`，开启了Spring集成测试支持。{![在Spring 4.2里，你可以选择基于规则的`SpringClassRule`和`SpringMethodRule`来代替`SpringJUnit4ClassRunner`。]}与此同时，`@ContextConfiguration`指定了如何加载应用程序上下文。此处我们让它加载`AddressBookConfiguration`里配置的Spring应用程序上下文。

除了加载应用程序上下文，`SpringJUnit4ClassRunner`还能通过自动织入从应用程序上下文里向测试本身注入Bean。因为这是一个针对`AddressService` Bean的测试，所以需要将它注入测试。最后，`testService()`方法调用地址服务并验证了结果。

虽然`@ContextConfiguration`在加载Spring应用程序上下文的过程中做了很多事情，但它没能加载完整的Spring Boot。Spring Boot应用程序最终是由`SpringApplication`加载的，可以显式加载（如代码清单2-1），这里也可以使用`SpringBootServletInitializer`（我们会在第8章里看到具体做法）。`SpringApplication`不仅加载应用程序上下文，还会开启日志、加载外部属性（application.properties或application.yml），以及其他Spring Boot特性。用`@ContextConfiguration`就得不到这些特性。

要在集成测试里获得这些特性，可以把`@ContextConfiguration`替换为Spring Boot的`@SpringApplicationConfiguration`：
```
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(
      classes=AddressBookConfiguration.class)
public class AddressServiceTests {
  ...
}
```
`@SpringApplicationConfiguration`的用法大体上和`@ContextConfiguration`一样，但也有不同的地方，`@SpringApplicationConfiguration`加载Spring应用程序上下文的方式同`SpringApplication`相同，处理方式和生产应用程序中的情况相同。这包括了加载外部属性和Spring Boot日志。

我们有充分的理由说，在大多数情况下，为Spring Boot应用程序编写测试时应该用`@SpringApplicationConfiguration`代替`@ContextConfiguration`。在本章中，我们当然也会用`@SpringApplicationConfiguration`来为Spring Boot应用程序编写测试，包括那些面向前端的应用程序。

说到Web测试，这正是我们接下来要做的。

## 4.2 测试Web应用程序

Spring MVC有一点好处：它的编程模型是围绕POJO展开的，在POJO上添加注解，声明如何处理Web请求。这种编程模型不仅简单，还让你能像对待应用程序中的其他组件一样对待这些控制器。你还可以针对这些控制器编写测试，就像测试POJO一样。

举例来说，考虑`ReadingListController`里的`addToReadingList()`方法：
```
@RequestMapping(method=RequestMethod.POST)
public String addToReadingList(Book book) {
  book.setReader(reader);
  readingListRepository.save(book);
  return "redirect:/readingList";
}
```
如果忽略`@RequestMapping`注解，你得到的就是一个相当基础的Java方法。你立马就能想到这样一个测试，提供一个`ReadingListRepository`的模拟实现，直接调用`addToReadingList()`，判断返回值并验证对`ReadingListRepository`的`save()`方法有过调用。

该测试的问题是，它仅仅测试了方法本身，当然，这要比没有测试好一点。然而，它没有测试该方法处理`/readingList`的`POST`请求的情况，也没有测试表单域绑定到`Book`参数的情况。虽然你可以判断返回的`String`包含特定值，但没法明确测试请求在方法处理完之后是否真的会重定向到`/readingList`。

要恰当地测试一个Web应用程序，你需要投入一些实际的HTTP请求，确认它能正确地处理那些请求。幸运的是，Spring Boot开发者有两个可选的方案能实现这类测试：

- *Spring Mock MVC*：能在一个近似真实的模拟Servlet容器里测试控制器，而不用实际启动应用服务器
- *Web integration tests*：在嵌入式Servlet容器（比如Tomcat或Jetty）里启动应用程序，在真正的应用服务器里执行测试

这两种方法各有利弊。很明显，启动一个应用服务器会比模拟Servlet容器要慢一些，但毫无疑问基于服务器的测试会更接近真实环境，更接近部署到生产环境运行的情况。

接下来，你会看到如何使用Spring Mock MVC测试框架来测试Web应用程序。然后，在4.3节里你会看到如何针对运行在应用服务器里的应用程序编写测试。

### 4.2.1 模拟Spring MVC

早在Spring 3.2，Spring Framework就有了一套非常实用的Web应用程序测试工具，能模拟Spring MVC，不需要真实的Servlet容器也能对控制器发送HTTP请求。Spring的Mock MVC框架模拟了Spring MVC的很多功能，让它几乎能和运行在Servlet容器里的应用程序一样，尽管实际并非如此。

要在你的测试里设置Mock MVC，可以使用`MockMvcBuilders`，该类提供了两个静态方法：

- `standaloneSetup()`：构建一个Mock MVC，提供一个或多个手工创建并配置的控制器。
- `webAppContextSetup()`：使用Spring应用程序上下文来构建Mock MVC，该上下文里可以包含一个或多个配置好的控制器。

两者的主要区别在于，`standaloneSetup()`希望你手工初始化并注入你要测试的控制器，而`webAppContextSetup()`则基于一个`WebApplicationContext`的实例，通常由Spring加载。前者同单元测试更加接近，你可能只想让它聚焦于单一控制器的测试，而后者让Spring来加载控制器及其依赖，以便进行完整的集成测试。

我们要用的是`webAppContextSetup()`。Spring完成了`ReadingListController`的初始化，并从Spring Boot自动配置的应用程序上下文里将其注入进来，我们直接对其进行测试。

`webAppContextSetup()`接受一个`WebApplicationContext`参数。因此，我们需要为测试类加上'@WebAppConfiguration'注解，使用`@Autowired`将`WebApplicationContext`作为实例变量注入测试类。以下代码演示了Mock MVC测试的执行入口。

__代码清单4-2 为集成测试控制器创建Mock MVC__

>原文P80~81代码

>**文字**

>Enables web context testing  开启Web上下文测试

>Injects WebApplicationContext：注入`WebApplicationContext`

>Sets up MockMvc：设置`MockMvc`

`@WebAppConfiguration`注解声明，由`SpringJUnit4ClassRunner`创建的应用程序上下文应该是一个`WebApplicationContext`（相对于基本的非Web`ApplicationContext`）。

`setupMockMvc()`方法上添加了JUnit的`@Before`注解，表明它应该在测试方法之前执行。它将`WebApplicationContext`注入`webAppContextSetup()`方法，然后调用`build()`产生了一个`MockMvc`实例，该实例赋给了一个实例变量，供测试方法使用。

现在我们有了一个`MockMvc`，已经可以开始写测试方法了。让我们先写个简单的测试方法，向`/readingList`发送一个HTTP `GET`请求，判断模型和视图是否满足我们的期望。下面的`homePage()`测试方法就是我们所需要的：

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

如你所见，我们在这个测试方法里使用了很多静态方法，包括Spring的`MockMvcRequestBuilders`和`MockMvcResultMatchers`里的静态方法，还有Hamcrest库的`Matchers`里的静态方法。在我们深入探讨这个测试方法前，先添加一些静态`import`，这样代码看起来更清爽一些：

```
import static org.hamcrest.Matchers.*;
import static org.springframework.test.web.servlet.request.
        ➥ MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.
        ➥ MockMvcResultMatchers.*;
```
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
现在这个测试方法读起来就很自然了。首先向`/readingList`发起一个`GET`请求，然后希望该请求处理成功（`isOk()`会判断HTTP 200响应码），并且视图的逻辑名称是`readingList`，测试还要断定模型包含一个名为`books`的属性，该属性是一个空集合。所有的断言都很直观。

值得一提的是，此处完全不需要将应用程序部署到Web服务器上，它是运行在模拟的Spring MVC中的，刚好能通过`MockMvc`实例处理我们给它的HTTP请求。

太酷了，不是吗？

让我们再来看一个测试方法，这次会更有趣一些，我们实际发送一个HTTP `POST`请求提交一本新书。我们应该期待`POST`请求处理后重定向回`/readingList`，模型中将包含新添加的图书。以下代码演示了如何通过Spring的Mock MVC来实现这个测试。

__代码清单4-3 测试提交一本新书__

>原书P82~83代码

>**文字**

>Performs POST request：执行POST请求

>Sets up expected book：配置期望的图书

>Performs GET request：执行GET请求

很明显，代码清单4-3里的测试更复杂一些，实际是两个测试放在一个方法里。第一部分提交图书并检查了请求的结果，第二部分执行了一次对主页的`GET`请求，检查新建的图书是否在模型中。

在提交图书时，我们必须确保将内容类型（通过`MediaType.APPLICATION_FORM_URLENCODED`）设置为application/x-www-form-urlencoded，这才是运行应用程序时浏览器会发送的内容类型。随后，用`MockMvcRequestBuilders`的`param`方法设置表单域，模拟要提交的表单。一旦请求执行，我们要检查响应是否是一个到`/readingList`的重定向。

假定以上测试都通过，我们进入第二部分。首先设置一个`Book`对象，包含想要的值。我们将用这个对象来和首页获取的模型的值进行对比。

随后对`/readingList`发起一个`GET`请求，大部分内容和我们之前测试主页时一样，只是之前模型中有一个空集合，而现在有一个集合项。这里要检查它的内容是否和我们创建的`expectedBook`一致。如此一来，我们的控制器看来保存了发送给它的图书，完成了工作。

至此，这些测试验证了一个未经保护的应用程序，和我们在第2章里写的应用程序很类似。但如果我们想要测试一个安全加固过的应用程序呢（比如我们在第3章里写的那个），又该怎么办？

### 4.2.2 测试Web安全

Spring Security能让你非常方便地测试安全加固后的Web应用程序。为了利用这点优势，你必须在项目里添加Spring Security的测试模块。要在Gradle里做到这一点，你所需要的就是以下`testCompile`依赖：

```
testCompile("org.springframework.security:spring-security-test")
```

如果你用的是Maven，则添加以下`<dependency>`：

```
<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-test</artifactId>
  <scope>test</scope>
</dependency>
```

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
`springSecurity()`方法返回了一个Mock MVC配置器，为Mock MVC开启了Spring Security支持。只需像上面这样运用就行了，Spring Security会介入`MockMvc`上执行的每个请求。具体的安全配置取决于你如何配置Spring Security（或者Spring Boot如何自动配置Spring Security）。在阅读列表这个应用程序里，这个配置与我们在第3章里创建的`SecurityConfig.java`里的配置是一样的。

>__`springSecurity()`方法__

>`springSecurity()`是`SecurityMockMvcConfigurers`的一个静态方法，考虑到可读性，我已经将其静态导入。

开启了Spring Security之后，在请求主页的时候，我们便不能只期待HTTP 200响应。如果请求未经身份验证，我们应该期待重定向到登录页面：
```
@Test
public void homePage_unauthenticatedUser() throws Exception {
  mockMvc.perform(get("/"))
    .andExpect(status().is3xxRedirection())
    .andExpect(header().string("Location",
                               "http://localhost/login"));
}
```
但是我们又该如何发起一个经过身份验证的请求呢？Spring Security提供了两个注解：

- `@WithMockUser`：加载安全上下文，其中包含一个`UserDetails`，使用了给定的用户名、密码和授权。
- `@WithUserDetails`：根据给定的用户名查找`UserDetails`对象，加载安全上下文。

在这两种情况下，Spring Security的安全上下文都会加载一个`UserDetails`对象，添加了该注解的测试方法运行过程中都会使用该对象。`@WithMockUser`注解是两者里比较基础的那个，它允许显式声明一个`UserDetails`并加载到安全上下文里：
```
@Test
@WithMockUser(username="craig",
              password="password",
              roles="READER")
public void homePage_authenticatedUser() throws Exception {
  ...
}
```
如你所见，`@WithMockUser`绕过了对`UserDetails`对象的正常查询，用给定的值创建了一个`UserDetails`对象取而代之。在简单的测视里，这就够用了。但我们的测试需要一个`Reader`（实现了`UserDetails`）而非`@WithMockUser`创建的通用`UserDetails`。为此，我们需要`@WithUserDetails`。

`@WithUserDetails`注解使用事先配置好的`UserDetailsService`来加载`UserDetails`对象。回想一下第3章，我们配置了一个`UserDetailsService` Bean，它会根据给定的用户名查找并返回一个`Reader`对象。太完美了！所以我们要为测试方法添加`@WithUserDetails`注解，就像下面的代码一样。

__代码清单4-4 测试带有用户身份验证的安全加固方法__

>原书P85代码

>**文字**

>Uses “craig” user ：使用craig用户

>Sets up expected Reader：配置期望的`Reader`

>Performs GET request：发起`GET`请求

在代码清单4-4里，我们通过`@WithUserDetails`注解声明了要在测试方法执行过程中向安全上下文里加载craig用户。`Reader`会放入模型之中，该测试方法先创建了一个期望的`Reader`对象，后续可以用来进行比较。随后发起`GET`请求，对视图名和模型内容做了断言，其中包括名为`reader`的模型属性。

同样，此处没有启动Servlet容器来运行这些测试，Spring的Mock MVC取代了实际的Servlet容器。这样做的好处是测试方法运行相对较快，因为不需要等待服务器启动，而且不需要打开Web浏览器发送表单，所以测试比较简单快捷。

另一方面，这并不是一个完整的测试。它比直接调用控制器方法要好，但它并没有真的在Web浏览器里执行应用程序，验证呈现出的视图。为此，我们需要启动一个真正的Web服务器，用真实浏览器来访问它。让我们来看看Spring Boot如何启动一个真实的Web服务器来帮助测试。

##4.3 测试运行中的应用程序

说到测试Web应用程序，我们还没接触实质内容。在真实的服务器里启动应用程序，用真实的Web浏览器访问它，这样比使用模拟的测试引擎更能表现应用程序在用户端的行为。

但是，用真实的Web浏览器在真实的服务器上运行测试会很麻烦。虽然有构建时的插件能把应用程序部署到Tomcat或者Jetty里，但它们配置起来多有不便。而且这么多测试几乎不可能隔离运行，或者不启动构建工具。

然而Spring Boot找到了解决方案。它支持将Tomcat或Jetty这样的嵌入式Servlet容器作为运行中的应用程序的一部分，可以运用相同的机制，在测试过程中用嵌入式Servlet容器来启动应用程序。

Spring Boot的`@WebIntegrationTest`注解就是这么做的。在测试类上添加`@WebIntegrationTest`注解，可以声明你不仅希望Spring Boot为测试创建应用程序上下文，还要启动一个嵌入式的Servlet容器。一旦应用程序运行在嵌入式容器里，你就可以发起真实的HTTP请求，对结果做断言了。

举例来说，考虑代码清单4-5里的那段简单的Web测试，它用`@WebIntegrationTest`，在服务器里启动了应用程序，使用Spring的`RestTemplate`对应用程序发起HTTP请求。

__代码清单4-5 测试运行在服务器里的Web应用程序__

>原书P86代码

>**文字**

>Runs test in server：在服务器里运行测试

>Performs GET request：发起`GET`请求

>Asserts HTTP 404 (not found) response：判断HTTP 404 (NOT FOUND)响应

虽然这个测试非常简单，但足以演示如何使用`@WebIntegrationTest`在服务器里启动应用程序。实际启动的服务器究竟是哪个，判断逻辑与在命令行里运行应用程序时一样。默认情况下，一个监听8080端口的Tomcat会启动。但是，如果Classpath里有的话，Jetty或者Undertow也能启动这些服务器。

测试方法的主体部分假设应用程序已经运行，监听了8080端口。它使用了Spring的`RestTemplate`对一个不存在的页面发起请求，判断服务器的响应是否是HTTP 404 (NOT FOUND)。如果返回了其他响应，则测试失败。

### 4.3.1 用随机端口启动服务器

前面提到过，此处的默认行为是启动服务器监听8080端口。在一台机器上一次只运行一个测试的话，这没什么问题，因为没有其他服务器监听8080端口。但如果你和我一样，本机总是有其他服务器在监听8080端口，那该怎么办？这时测试就会失败，因为端口冲突，服务器启动不了。一定要有更好的办法才行。

幸运的是，让Spring Boot在随机选择的端口上启动服务器很方便。一种办法是将`server.port`属性设置为`0`，让Spring Boot选择一个随机的可用端口。`@WebIntegrationTest`的`value`属性接受一个`String`数组，数组中的每项都是键值对，形如`name=value`，用来设置测试中使用的属性。要设置`server.port`，你可以这样做：
```
@WebIntegrationTest(value={"server.port=0"})
```
或者，因为只要设置一个属性，所以还能有更简单的形式：
```
@WebIntegrationTest("server.port=0")
```
通过`value`属性来设置属性通常还算方便，但`@WebIntegrationTest`还提供了一个`randomPort`属性，更明确地表示让服务器在随机端口上启动。你可以将`randomPort`设置为`true`，启用随机端口：
```
@WebIntegrationTest(randomPort=true)
```
既然我们在随机端口上启动了服务器，就需要在发起Web请求时确保使用正确的端口。此时的`getForObject()`方法在URL里硬编码了8080端口。如果端口是随机选择的，那在构造请求时又该怎么确定正确的端口呢？

首先，我们需要以实例变量的形式注入选中的端口。为了方便起见，Spring Boot将`local.server.port`的值设置为了选中的端口。我们只需使用Spring的`@Value`注解将其注入即可：
```
@Value("${local.server.port}")
private int port;
```
有了端口之后，只需对`getForObject()`稍作修改，使用这个`port`就好了：
```
rest.getForObject(
    "http://localhost:{port}/bogusPage", String.class, port);
```
这里我们在URL里把硬编码的8080改为`{port}`占位符。在`getForObject()`调用里把`port`属性作为最后一个参数传入，就能确保该占位符被替换为注入`port`的值了。

### 4.3.2 使用Selenium测试HTML页面

`RestTemplate`对于简单的请求而言很好用，也是测试REST端点的理想工具。但是，就算它能对返回HTML页面的URL发起请求，也不方便对页面内容或者页面上执行的操作进行断言。结果HTML里的内容最好可以精确判断（这种测试很脆弱）。不过你无法方便地判断页面上选中的内容，或者执行诸如点击链接或提交表单这样的操作。

对于HTML应用程序测试，有一个更好的选择——Selenium（[www.seleniumhq.org](http://www.seleniumhq.org)），它的功能远不止为你提交请求并获取结果，它能实际打开一个Web浏览器，在浏览器的上下文中执行你的测试。Selenium能尽量接近手动执行测试，但与手工测试不同，Selenium的测试是自动的，而且可以重复运行。

为了用Selenium测试阅读列表应用程序，让我们先写一个测试来获取首页，为新书填写表单，提交表单，随后判断返回的页面里是否包含新添加的图书。

首先需要把Selenium作为测试依赖添加到项目里：
```
testCompile("org.seleniumhq.selenium:selenium-java:2.45.0")
```
现在就可以编写测试了，以下代码是一个基本的Selenium测试模板，使用了Spring Boot的`@WebIntegrationTest`。

__代码清单4-6 在Spring Boot里使用Selenium测试的模板__

>原书P88~89代码

>**文字**

>Starts on a random port：用随机端口启动

>Injects the port：注入端口号

>Sets up Firefox driver：配置Firefox驱动

>Shuts down browser：关闭浏览器

和我们之前写的更简单的Web测试一样，这个类添加了`@WebIntegrationTest`注解，将`randomPort`设置为`true`，这样应用程序启动后会运行一个监听随机端口的服务器。同样，端口号注入`port`属性，这样我们就能用它来构造指向运行中的应用程序的URL了。

静态方法`openBrowser()`会创建一个`FirefoxDriver`的实例，它将打开一个Firefox浏览器（需要在运行测试的服务器上安装该浏览器）。我们的测试方法将通过`FirefoxDriver`实例来执行浏览器操作。在页面上查找元素时，`FirefoxDriver`配置了10秒的等候时间（以防那些元素加载过慢）。

测试执行完毕后，我们需要关闭Firefox浏览器，因此在`closeBrowser()`里要调用`FirefoxDriver`实例的`quit()`方法，关闭浏览器。

>__选择浏览器__ ：虽然我们用Firefox进行了测试，但Selenium还提供了不少其他浏览器的驱动，包括IE、Google的Chrome，还有Apple的Safari。你的测试不仅可以使用其他浏览器，还能使用各种你想支持的浏览器，这也许也是个不错的想法。

现在可以开始编写我们的测试方法了，给你提个醒，我们想要加载首页，填充并发送表单，然后判断我们登录到的页面上是否包含刚刚添加的新书。下面的代码演示了如何用Selenium实现这个功能。

__代码清单4-7 用Selenium测试阅读列表应用程序__

>原书P89~90代码

>**文字**

>Fetches the home page：获取主页

>Asserts an empty book list：判断图书列表是否为空

>Fills in and submits form：填充并发送表单

>Asserts new book in list：判断列表中是否包含新书

该测试方法做的第一件事是使用`FirefoxDriver`来发起`GET`请求，获取阅读列表的主页，随后查找页面里的一个`<div>`元素，从它的文本里判断列表里没有图书。

接下来的几行查找表单里的元素，使用驱动的`sendKeys()`方法模拟敲击键盘事件（实际上就是用给定的值填充那些表单域）。最后，找到`<form>`元素并提交。

提交的表单经处理后，浏览器就会跳到一个页面，上面的列表里包含了新添加的图书。因此最后几行查找列表里的`<dt>`和`<dd>`元素，判断其中是否包含测试表单里提交的数据。

运行测试时，你会看到浏览器打开，加载了阅读列表应用程序。如果你够仔细，还会看到填充表单的过程，就好像幽灵在操作，当然，并没有幽灵在使用你的应用程序——这只是一个测试。

这个测试里最值得注意的是，`@WebIntegrationTest`可以为我们启动应用程序和服务器，这样Selenium才可以用Web浏览器执行测试。但真正有趣的是你可以使用IDE的测试功能来运行测试，随便你想跑几次都行，无需依赖构建过程中的某些插件来为你启动服务器。

要是你觉得使用Selenium进行测试很实用，可以阅读Yujun Liang和Alex Collins的*Selenium WebDriver in Practice*（[http://manning.com/liang/](http://manning.com/liang/)），该书更深入地讨论了Selenium测试的细节。

## 4.4 小结

测试是开发高质量软件的重要一环，没有好的测试，你永远都不无法保证应用程序能像你期望的那样运行。

单元测试专注于单一组件或组件中的一个方法，此处并不一定要用Spring。Spring提供了一些好处和技术——松耦合、依赖注入和接口驱动设计，这些都简化了单元测试编写。但Spring不用直接涉足单元测试。

集成测试会涉及众多组件，这时就要靠Spring来帮忙了。实际上，如果在运行时Spring负责拼装那些组件，那么在集成测试里Spring同样应该肩负这一职责。

Spring Framework以JUnit类运行器的方式提供了集成测试支持，它会加载Spring应用程序上下文，把上下文里的Bean注入测试。Spring Boot在Spring的集成测试之上又增加了配置加载器，以Spring Boot的方式加载应用程序上下文，包括了对外置属性的支持和Spring Boot日志。

Spring Boot还支持容器内测试Web应用程序，让你能用和生产环境一样的容器来启动应用程序。这让测试在验证应用程序行为的时候，更接近真实的运行环境。

此时我们已经构建了一个相当完整的应用程序（虽然有点简单），它利用Spring Boot的起步依赖和自动配置来处理低级的工作，让我们能专心开发自己的应用程序。我们也看到了如何使用Spring Boot的支持来测试应用程序。在后续几章里，我们会看到一些不同的东西，了解让Spring Boot应用程序开发更加简单的Groovy。在第5章，我们会先了解一些Grails框架特性，看看它们在Spring Boot中的用途。
