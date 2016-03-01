# Customizing configuration
# 自定义配置

__This chapter covers__  
__本章内容涉及__

* Overriding auto-configured beans  
覆盖自动配置的Bean
* Configuring with external properties  
用外置属性进行配置
* Customizing error pages  
自定义错误页

Freedom of choice is an awesome thing. If you’ve ever ordered a pizza (who hasn’t?) then you know that you have full control over what toppings are placed on the pie. If you ask for sausage, pepperoni, green peppers, and extra cheese, then you’re essentially configuring the pizza to your precise specifications.  
能自由选择真是太棒了，如果你曾订过披萨（有没订过的么？）就会知道自己能完全掌控薄饼上放哪些辅料。如果你要加腊肠、意大利辣香肠、青辣椒和额外的芝士，那么你就是在配置披萨，把它变成针对你个人喜好的披萨。

On the other hand, most pizza places also offer a form of auto-configuration. You can ask for the meat-lover’s pizza, the vegetarian pizza, the spicy Italian pizza, or the ultimate example of pizza auto-configuration, the supreme pizza. When ordering one of these pizzas, you don’t have to explicitly specify the toppings. The type of pizza ordered implies what toppings are used.  
另一方面，大部分披萨店也提供某种形式的自动配置，你可以点肉食者的披萨、素食者的披萨、香辣意大利披萨，或者是自动配置披萨中的极品——至尊披萨（supreme pizza）。在下单时，你并没有指定具体的辅料，你所点的披萨种类决定了所用的辅料。

But what if you like all of the toppings of the supreme pizza, but also want jalapenos and would rather not have mushrooms? Does your taste for spicy food and aversion to fungus mean that auto-configuration isn’t applicable and that you must explicitly configure your pizza? Absolutely not. Most pizzerias will let you customize your pizza, even if you started with a preconfigured option from the menu.  
但如果你想要至尊披萨上的全部辅料，还想要加墨西哥胡椒，但又不想放蘑菇该怎么办？你偏爱辣食又不喜欢吃菌类，也就是说自动配置不适合你的口味，你就只能自己搭配披萨了么？当然不是，大部分披萨店会让你做些定制，以菜单上已有的选项作为基础。

Working with traditional Spring configuration is much like ordering a pizza and explicitly specifying all of the toppings. You have full control over what goes into your Spring configuration, but explicitly declaring all of the beans in the application is non-optimal. On the other hand, Spring Boot auto-configuration is like ordering a specialty pizza from the menu. It’s easier to let Spring Boot handle the details than to declare each and every bean in the application context.  
使用传统Spring配置的过程和订披萨很像，你要自己指定全部的辅料。你可以完全掌控Spring配置的内容，可是显式声明应用程序里全部的Bean并不是明智之举。而Spring Boot自动配置就像是从菜单上选份特定的披萨，让Spring Boot处理各种细节要比自己声明上下文里全部的Bean要容易很多。

Fortunately, Spring Boot auto-configuration is flexible. Like the pizzeria that will leave off the mushrooms and add jalapenos to your pizza, Spring Boot will let you step in and influence how it applies auto-configuration.  
幸运的是Spring Boot自动配置非常灵活，就像披萨厨师在你的披萨里不放蘑菇加墨西哥胡椒一样，Spring Boot能让你介入进来，影响自动配置的实施。

In this chapter, we’re going to look at two ways to influence auto-configuration: explicit configuration overrides and fine-grained configuration with properties. We’ll also look at how Spring Boot has provided hooks for you to plug in a custom error page.  
本章我们将看到两种影响自动配置的方式——使用显式配置进行覆盖和使用属性进行精细化配置，还会看到如何使用Spring Boot提供的钩子引入自定义的错误页。

## 3.1 Overriding Spring Boot auto-configuration
## 3.1 覆盖Spring Boot自动配置

Generally speaking, if you can get the same results with no configuration as you would with explicit configuration, no configuration is the no-brainer choice. Why would you do extra work, writing and maintaining extra configuration code, if you can get what you need without it?  
一般来说，如果不用配置就能得到和显式配置一样的结果，那么不写配置是最直接的选择。既然如此，那干嘛还要多做额外的工作呢？如果不用编写和维护额外的配置代码也行，那何必还要它们呢。

Most of the time, the auto-configured beans are exactly what you want and there’s no need to override them. But there are some cases where the best guess that Spring Boot can make during auto-configuration probably isn’t going to be good enough.  
大多数情况下，自动配置的Bean刚好能满足你的需要，不需要去覆盖它们。但也有些情况，Spring Boot在自动配置时的推断还不够好。

A prime example of a case where auto-configuration isn’t good enough is when you’re applying security to your application. Security is not one-size-fits-all, and there are decisions around application security that Spring Boot has no business making for you. Although Spring Boot provides some basic auto-configuration for security, you’ll certainly want to override it to meet your specific security requirements.  
这里有个好例子，当你在应用程序里添加安全特性时自动配置做的还不够好。安全配置并不是放之四海而皆准的，围绕应用程序安全有很多决策要做，Spring Boot不能替你做决定。虽然Spring Boot为安全提供了一些基本的自动配置，但是你还是需要自己覆盖一些配置以满足自己的特定安全要求。

To see how to override auto-configuration with explicit configuration, we’ll start by adding Spring Security to the reading-list example. After seeing what you get for free with auto-configuration, we’ll then override the basic security configuration to fit a particular situation.  
想知道如何用显式的配置来覆盖自动配置，我们先从为阅读列表应用程序添加Spring Security入手。在了解了自动配置提供了什么之后，我们再来覆盖基础的安全配置，以满足特定的场景需求。

### 3.1.1 Securing the application
### 3.1.1 保护应用程序

Spring Boot auto-configuration makes securing an application a piece of cake. All you need to do is add the security starter to the build. For Gradle, the following dependency will do:  
Spring Boot自动配置让应用程序的安全工作变得易如反掌，你要做的只是添加security起步依赖。以Gradle为例，添加如下依赖：

```
compile("org.springframework.boot:spring-boot-starter-security")
```

Or, if you’re using Maven, add this <dependency> to your build’s <dependencies> block:  
如果你使用的是Maven，在项目的`<dependencies>`块中加入如下`<dependency>`：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

That’s it! Rebuild your application and run it. It’s now a secure web application! The security starter adds Spring Security (among other things) to the application’s classpath. With Spring Security on the classpath, auto-configuration kicks in and a very basic Spring Security setup is created.  
这样就搞定了！重新构建应用程序后运行即可，现在这就是一个安全的Web应用程序了！security起步依赖在应用程序的Classpath里添加了Spring Secuirty（和其他一些东西）。Classpath里有Spring Security后，自动配置就能介入其中创建一个基本的Spring Security配置。

If you try to open the application in your browser, you’ll be immediately met with an HTTP Basic authentication dialog box. The username you’ll need to enter is “user”. As for the password, it’s a bit trickier. The password is randomly generated and written to the logs each time the application is run. You’ll need to look through the logging messages (written to stdout by default) and look for a line that looks something like this:  
如果你试着在浏览器里打开该应用程序，马上就会看到HTTP基础身份验证对话框。此处的用户名是“user”，至于密码，就有点麻烦了。密码是在应用程序每次运行时随机生成后写入日志的，你需要查找日志消息（默认写入STDOUT），找到与以下例子类似的内容：

```
Using default security password: d9d8abe5-42b5-4f20-a32a-76ee3df658d9
```

I can’t say for certain, but I’m guessing that this particular security setup probably isn’t ideal for you. First, HTTP Basic dialog boxes are clunky and not very user-friendly. And I’ll bet that you don’t develop too many applications that have only one user who doesn’t mind looking up their password from a log file. Therefore, you’ll probably want to make a few changes to how Spring Security is configured. At very least, you’ll want to provide a nice-looking login page and specify an authentication service that operates against a database or LDAP-based user store.  
我不能肯定，但我猜这个特定的安全配置并不是你的理想选择。首先，HTTP基础身份验证对话框有点粗糙，对用户并不友好。而且，我敢打赌你不会开发这种只有一个用户的应用程序，他还要从日志文件里找到自己的密码。因此，你会希望修改一些Spring Security的配置，至少像要有一个好看一些的登录页，还要有一个基于数据库或LDAP的身份验证服务。

Let’s see how to do that by writing some explicit Spring Security configuration to override the auto-configured security scheme.  
让我们看看如何写一些Spring Secuirty配置来覆盖自动配置的安全设置吧。

### 3.1.2 Creating a custom security configuration
### 3.1.2 创建自定义的安全配置

Overriding auto-configuration is a simple matter of explicitly writing the configuration as if auto-configuration didn’t exist. This explicit configuration can take any form that Spring supports, including XML configuration and Groovy-based configuration.  
覆盖自动配置很简单，就当自动配置不存在，直接显式地写一段配置。这段显式配置的形式不限，Spring支持的XML和Groovy形式配置都可以。

For our purposes, we’re going to focus on Java configuration when writing explicit configuration. In the case of Spring Security, this means writing a configuration class that extends WebSecurityConfigurerAdapter. SecurityConfig in listing 3.1 is the configuration class we’ll use.  
在编写配置时，我们会专注于Java形式的配置。在Spring Security的场景下，这意味着写一个扩展了`WebSecurityConfigurerAdapter`的配置类。代码3.1里的`SecurityConfig`就是我们需要的东西。

__Listing 3.1 Explicit configuration to override auto-configured security__  
__代码3.1 覆盖自动配置的显式安全配置__

```
package readinglist;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.
                                    builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.
                                                        HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.
                                                        EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.
                                            WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private ReaderRepository readerRepository;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/").access("hasRole('READER')")
                .antMatchers("/**").permitAll()
            .and()
            .formLogin()
                .loginPage("/login")
                .failureUrl("/login?error=true");
    }

    @Override
    protected void configure(
                              AuthenticationManagerBuilder auth) throws Exception {
        auth
            .userDetailsService(new UserDetailsService() {
                @Override
                public UserDetails loadUserByUsername(String username)
                        throws UsernameNotFoundException {
                    return readerRepository.findOne(username);
                }
            });
    }
}
```

Require READER access  
要求登录者有READER角色

Set login form path  
设置登录表单的路径

Define custom UserDetailsService  
定义自定义`UserDetailsService`

SecurityConfig is a very basic Spring Security configuration. Even so, it does a lot of what we need to customize security of the reading-list application. By providing this custom security configuration class, we’re asking Spring Boot to skip security auto-configuration and to use our security configuration instead.  
`SecurityConfig`是个非常基础的Spring Security配置，尽管如此，它还是完成了不少安全定制工作。通过这个自定义的安全配置类，我们让Spring Boot跳过了安全自动配置，转而使用我们的安全配置。

Configuration classes that extend WebSecurityConfigurerAdapter can override two different configure() methods. In SecurityConfig, the first configure() method specifies that requests for “/” (which ReadingListController’s methods are mapped to) require an authenticated user with the READER role. All other request paths are configured for open access to all users. It also designates /login as the path for the login page as well as the login failure page (along with an error attribute).  
扩展了`WebSecurityConfigurerAdapter`的配置类可以覆盖两个不同的`configure()`方法，在`SecurityConfig`里，第一个`configure()`方法指明“/”（`ReadingListController`的方法映射到了该路径）的请求只有经过身份认证且拥有READER角色的用户才能访问。其他的所有请求路径向所有用户开放了访问权限。这里还将登录页和登录失败页（带有一个`error`属性）指定到了/login。

Spring Security offers several options for authentication, including authentication against JDBC-backed user stores, LDAP-backed user stores, and in-memory user stores. For our application, we’re going to authenticate users against the database via JPA. The second configure() method sets this up by setting a custom user details service. This service can be any class that implements UsersDetailsService and is used to look up user details given a username. The following listing has given it an anonymous inner-class implementation that simply calls the findOne() method on an injected ReaderRepository (which is a Spring Data JPA repository interface).  
Spring Security为身份认证提供了众多选项，后端可以是JDBC、LDAP和内存用户存储。我们的应用程序中，我们会通过JPA是用数据库来存储用户信息。第二个`configure()`方法设置了一个自定义的`UserDetailsService`，这个服务可以是任意实现了`UserDetailsService`的类，用于查找指定用户名的用户。如下代码提供了一个匿名内部类实现，简单地调用了注入的`ReaderRepository`（这是一个Spring Data JPA仓库接口）的`findOne()`方法。

__Listing 3.2 A repository interface for persisting readers__  
__代码3.2 用来持久化读者信息的仓库接口__

```
package readinglist;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ReaderRepository
                     extends JpaRepository<Reader, String> {
}
```

Persist readers via JPA  
通过JPA持久化读者

As with BookRepository, there’s no need to write an implementation of ReaderRepository. Because it extends JpaRepository, Spring Data JPA will automatically
create an implementation of it at runtime. This affords you 18 methods for working with Reader entities.  
和`BookRepository`类似，无需自己实现`ReaderRepository`，因为它扩展了`JpaRepository`，Spring Data JPA会在运行时自动创建它的实现，这为你提供了18个操作`Reader`实体的方法。

Speaking of Reader entities, the Reader class (shown in listing 3.3) is the final piece of the puzzle. It’s a simple JPA entity type with a few fields to capture the username, password, and full name of the user.  
说到`Reader`实体，`Reader`类（如代码3.3所示）就是最后一块拼图了，它就是一个简单的JPA实体，其中有几个字段用来存储用户名、密码和用户全名。

__Listing 3.3 A JPA entity that defines a Reader__
__代码3.3 定义Reader的JPA实体__

```
package readinglist;

import java.util.Arrays;
import java.util.Collection;
import javax.persistence.Entity;
import javax.persistence.Id;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

@Entity
public class Reader implements UserDetails {

  private static final long serialVersionUID = 1L;

  @Id
  private String username;
  private String fullname;
  private String password;

  public String getUsername() {
    return username;
  }

  public void setUsername(String username) {
    this.username = username;
  }

  public String getFullname() {
    return fullname;
  }

  public void setFullname(String fullname) {
    this.fullname = fullname;
  }

  public String getPassword() {
    return password;
  }

  public void setPassword(String password) {
    this.password = password;
  }

  // UserDetails methods
  @Override
  public Collection<? extends GrantedAuthority> getAuthorities() {
    return Arrays.asList(new SimpleGrantedAuthority("READER"));
  }

  @Override
  public boolean isAccountNonExpired() {
    return true;
  }

  @Override
  public boolean isAccountNonLocked() {
    return true;
  }

  @Override
  public boolean isCredentialsNonExpired() {
    return true;
  }

  @Override
  public boolean isEnabled() {
    return true;
  }
}
```

Reader fields  
Reader字段

Grant READER privilege  
授予READER权限

Do not expire, lock, or disable  
不过期，不加锁，不禁用

As you can see, Reader is annotated with @Entity to make it a JPA entity. In addition, its username field is annotated with @Id to designate it as the entity’s ID. This seemed like a natural choice, as the username should uniquely identify the Reader.  
如你所见，`Reader`用了`@Entity`注解，所以这是一个JPA实体。此外，它的`username`字段上有`@Id`注解，表明这是实体的ID。这个选择无可厚非，因为`username`应该能唯一标识一个`Reader`。

You’ll also notice that Reader implements the UserDetails interface and several of its methods. This makes it possible to use a Reader object to represent a user in Spring Security. The getAuthorities() method is overridden to always grant users READER authority. The isAccountNonExpired(), isAccountNonLocked(), isCredentialsNonExpired(), and isEnabled() methods are all implemented to return true so that the reader account is never expired, locked, or revoked.  
你应该还注意到`Reader`实现了`UserDetails`接口中的方法，这样`Reader`就能代表Spring Security里的用户了。`getAuthorities()`方法被覆盖过了，始终会为用户授予READER权限。`isAccountNonExpired()`、 `isAccountNonLocked()`、`isCredentialsNonExpired()`和`isEnabled()`方法都返回`true`
，这样读者账户就不会过期，不会被锁定，也不会被撤销。

Rebuild and restart the application and you should be able to log in to the application as one of the readers.  
重新构建并重启应用程序后，你应该就能以读者身份登录应用程序了。

> KEEPING IT SIMPLE In a larger application, the authorities granted to a user might themselves be entities and be maintained in a separate database table. Likewise, the boolean values indicating whether an account is non-expired, non-locked, and enabled might be fields drawn from the database. For our purposes, however, I’ve decided to keep these details simple so as not to distract from what it is we’re really discussing ... namely, overriding Spring Boot auto-configuration.  
__保持简单__ 在一个大型应用程序里，赋予用户的授权本身也可能是实体，它们被维护在独立的数据表里。同样的，表示一个账户是否是非过期、非锁定且可用的`boolean`值也是数据库里的字段。但是，出于演示考虑，我决定让这些细节保持简单，不至于转移注意力——我说的是覆盖Spring Boot自动配置。

There’s a lot more we could do with regard to security configuration,1 but this is all we need here, and it does demonstrate how to override the security auto-configuration provided by Spring Boot.  
在安全配置方面，我们还能做更多事情，<sup>[1][]</sup>但此刻这样就足够了，上面的例子足以演示如何覆盖Spring Boot提供的安全自动配置了。

Again, all you need to do to override Spring Boot auto-configuration is to write explicit configuration. Spring Boot will see your configuration, step back, and let your configuration take precedence. To understand how this works, let’s take a look under the covers of Spring Boot auto-configuration to see how it works and how it allows itself to be overridden.  
再重申一次，想要覆盖Spring Boot的自动配置，你所要做的就是编写一个显式的配置而已。Spring Boot会发现你的配置，随后降低自动配置的优先级，以你的配置为准。想弄明白这是如何实现的，让我们揭开Spring Boot自动配置的神秘面纱，看看它是如何运作的，它是怎么允许自己被覆盖的。

[1]: # "For a deeper dive into Spring Security, have a look at chapters 9 and 14 of my Spring in Action, Fourth Edition (Manning, 2014).想要深入了解Spring Security，可以参考《Spring in Action》第四版（Manning，2014）中的第9章和第14章。"

### 3.1.3 Taking another peek under the covers of auto-configuration
### 3.1.3 掀开自动配置的神秘面纱

As we discussed in section 2.3.3, Spring Boot auto-configuration comes with several configuration classes, any of which can be applied in your application. All of this configuration uses Spring 4.0’s conditional configuration support to make runtime decisions as to whether or not Spring Boot’s configuration should be used or ignored.  
正如我们在2.3.3节里讨论的那样，Spring Boot自动配置自带了很多配置类，每一个都能运用在你的应用程序里。它们都使用了Spring 4.0的条件化配置，可以在运行时判断这个配置是该被运用，还是该被忽略。

For the most part, the @ConditionalOnMissingBean annotation described in table 2.1 is what makes it possible to override auto-configuration. The JdbcTemplate bean defined in Spring Boot’s DataSourceAutoConfiguration is a very simple example of how @ConditionalOnMissingBean works:  
大部分情况下，表2.1里的`@ConditionalOnMissingBean`注解是覆盖自动配置的关键。Spring Boot的`DataSourceAutoConfiguration`中定义的`JdbcTemplate` Bean就是一个非常简单的例子，演示了`@ConditionalOnMissingBean`是如何工作的：

```
@Bean
@ConditionalOnMissingBean(JdbcOperations.class)
public JdbcTemplate jdbcTemplate() {
  return new JdbcTemplate(this.dataSource);
}
```

The jdbcTemplate() method is annotated with @Bean and is ready to configure a JdbcTemplate bean if needed. But it’s also annotated with @ConditionalOnMissingBean, which requires that there not already be a bean of type JdbcOperations (the interface that JdbcTemplate implements). If there’s already a JdbcOperations bean, then the condition will fail and the jdbcTemplate() bean method will not be used.  
`jdbcTemplate()`方法上添加了`@Bean`注解，在需要时可以配置出一个`JdbcTemplate` Bean。但它上面还加了`@ConditionalOnMissingBean`注解，要求当前不存在`JdbcOperations`类型（`JdbcTemplate`实现了该接口）的Bean时才生效。如果当前已经有一个`JdbcOperations` Bean了，条件即不满足，不会执行`jdbcTemplate()`方法。

What circumstances would result in there already being a JdbcOperation bean? Spring Boot is designed to load application-level configuration before considering its auto-configuration classes. Therefore, if you’ve already configured a JdbcTemplate bean, then there will be a bean of type JdbcOperations by the time that auto-configuration takes place, and the auto-configured JdbcTemplate bean will be ignored.  
什么情况下会存在一个`JdbcOperations` Bean呢？Spring Boot的设计是加载应用级配置在线，随后再考虑自动配置类。因此，如果你已经配置了一个`JdbcTemplate` Bean，那么在执行自动配置时就已经存在一个`JdbcOperations`类型的Bean了，于是忽略自动配置的`JdbcTemplate` Bean。

As it pertains to Spring Security, there are several configuration classes considered during auto-configuration. It would be impractical to go over each of them in detail here, but the one that’s most significant in allowing us to override Spring Boot’s auto- configured security configuration is SpringBootWebSecurityConfiguration. Here’s an excerpt from that configuration class:  
关于Spring Security，自动配置时会考虑几个配置类。在这里讨论每个配置类的细节是不切实际的，但覆盖Spring Boot自动配置的安全配置时最重要的一个类是`SpringBootWebSecurityConfiguration`。以下是其中的一个代码片段：

```
@Configuration
@EnableConfigurationProperties
@ConditionalOnClass({ EnableWebSecurity.class })
@ConditionalOnMissingBean(WebSecurityConfiguration.class)
@ConditionalOnWebApplication
public class SpringBootWebSecurityConfiguration {

  ...

}
```

As you can see, SpringBootWebSecurityConfiguration is annotated with a few conditional annotations. Per the @ConditionalOnClass annotation, the @EnableWebSecurity annotation must be available on the classpath. And per @ConditionalOnWebApplication, the application must be a web application. But it’s the @ConditionalOnMissingBean annotation that makes it possible for our security configuration class to be used instead of SpringBootWebSecurityConfiguration.  
如你所见，`SpringBootWebSecurityConfiguration`上加了好几个注解。看到`@ConditionalOnClass`注解后，你就应该知道Classpath里必须要有`@EnableWebSecurity`注解。`@ConditionalOnWebApplication`说明这必须是个Web应用程序。`@ConditionalOnMissingBean`注解才是我们的安全配置类代替`SpringBootWebSecurityConfiguration`的关键所在。

The @ConditionalOnMissingBean requires that there not already be a bean of type WebSecurityConfiguration. Although it may not be apparent on the surface, by annotating our SecurityConfig class with @EnableWebSecurity, we’re indirectly creating a bean of type WebSecurityConfiguration. Therefore, by the time auto-configuration takes place, there will already be a bean of type WebSecurityConfiguration, the @ConditionalOnMissingBean condition will fail, and any configuration offered by SpringBootWebSecurityConfiguration will be skipped over.  
`@ConditionalOnMissingBean`注解要求当下没有`WebSecurityConfiguration`类型的Bean。虽然表面上我们并没有这么一个Bean，但通过在`SecurityConfig`上添加`@EnableWebSecurity`注解，我们实际上间接创建了一个`WebSecurityConfiguration` Bean。所以在自动配置时，就已经存在这么一个Bean了，`@ConditionalOnMissingBean`条件不成立，`SpringBootWebSecurityConfiguration`提供的配置就被跳过了。

Although Spring Boot’s auto-configuration and @ConditionalOnMissingBean make it possible for you to explicitly override any of the beans that would otherwise be auto-configured, it’s not always necessary to go to that extreme. Let’s see how you can set a few simple configuration properties to tweak the auto-configured components.  
虽然Spring Boot的自动配置和`@ConditionalOnMissingBean`让你能显式地覆盖那些可以自动配置的Bean，但并不是每次都要做到这种程度的。让我们来看看怎么通过设置几个简单的配置就能调整自动配置组件吧。

## 3.2 Externalizing configuration with properties
## 3.2 通过属性文件外置配置

When dealing with application security, you’ll almost certainly want to take full charge of the configuration. But it would be a shame to give up on auto-configuration just to tweak a small detail such as a server port number or a logging level. If you need to set a database URL, wouldn’t it be easier to set a property somewhere than to completely declare a data source bean?  
在处理应用安全时，你当然会希望完全掌控所有配置。但也不必放弃自动配置，只微调一些细节也不是什么害羞的事，比如改改端口号和日志级别。要设置数据库URL时，是在哪里配置一个属性
简单，还是完整地声明一个数据源的Bean简单，答案不言而喻？

As it turns out, the beans that are automatically configured by Spring Boot offer well over 300 properties for fine-tuning. When you need to adjust the settings, you can specify these properties via environment variables, Java system properties, JNDI, command-line arguments, or property files.  
事实上，Spring Boot自动配置的Bean提供了超过300个用于微调的属性。当你要调整设置时，只要再环境变量、Java系统属性、JNDI、命令行参数或者属性文件里进行指定就好了。

To get started with these properties, let’s look at a very simple example. You may have noticed that Spring Boot emits an ascii-art banner when you run the reading-list application from the command line. If you’d like to disable the banner, you can do so by setting a property named spring.main.show-banner to false. One way of doing that is to specify the property as a command-line parameter when you run the app:  
要了解这些属性，让我们来看个非常简单的例子。你也许已经注意到了，在命令行里运行阅读列表应用程序时，Spring Boot有一个ascii-art Banner。如果你想禁用这个Banner，可以将`spring.main.show-banner`这个属性设置为`false`，有几种实现方式，其中之一就是在运行应用程序的命令行参数里指定：

```
$ java -jar readinglist-0.0.1-SNAPSHOT.jar --spring.main.show-banner=false
```

Another way is to create a file named application.properties that includes the following line:  
另一种方式是创建一个名为application.properties的文件，包含如下内容：

```
spring.main.show-banner=false
```

Or, if you’d prefer, create a YAML file named application.yml that looks like this:  
或者，如果你喜欢的话，也可以创建名为application.yml的YAML文件，内容如下：

```
spring:
  main:
  show-banner: false
```

You could also set the property as an environment variable. For example, if you’re using the bash or zsh shell, you can set it with the export command:  
还可以将属性设置为环境变量。举例来说，如果你用的是bash或者zsh，可以用`export`命令：

```
$ export spring_main_show_banner=false
```

Note the use of underscores instead of periods and dashes, as required for environment variable names.  
请注意，这里用的还下划线而不是点和横杠，这是对环境变量名称的要求。

There are, in fact, several ways to set properties for a Spring Boot application. Spring Boot will draw properties from several property sources, including the following:  
实际上有好几种设置Spring Boot应用程序的途径，Spring Boot能从多种属性源获得属性，包括：

1. Command-line arguments  
命令行参数
2. JNDI attributes from java:comp/env  
java:comp/env里的JNDI属性
3. JVM system properties  
JVM系统属性
4. Operating system environment variables  
操作系统环境变量
5. Randomly generated values for properties prefixed with random.* (referenced when setting other properties, such as `${random.long}`)  
随机生成的带random.* 前缀的属性（在设置其他属性时，可以引用它们，比如`${random.long}`）
6. An application.properties or application.yml file outside of the application  
应用程序以外的application.properties或者appliaction.yml文件
7. An application.properties or application.yml file packaged inside of the application  
打包在应用程序内的application.properties或者appliaction.yml文件
8. Property sources specified by @PropertySource  
通过`@PropertySource`标注的属性源
9. Default properties  
默认属性

This list is in order of precedence. That is, any property set from a source higher in the list will override the same property set on a source lower in the list. Command-line arguments, for instance, override properties from any other property source.  
这个列表的按照优先级排序，也就是说，任何在高优先级属性源里设置的属性都会覆盖低优先级的相同属性。例如，命令行参数会覆盖其他属性源。

As for the application.properties and application.yml files, they can reside in any of four locations:  
对application.properties和application.yml文件而言，它们能放在以下四个位置：

1. Externally, in a /config subdirectory of the directory from which the application is run  
外置，在/config子目录，相对于应用程序运行的目录
2. Externally, in the directory from which the application is run  
外置，在应用程序运行的目录里
3. Internally, in a package named “config”  
内置，在“config”包内
4. Internally, at the root of the classpath  
内置，在Classpath根目录

Again, this list is in order of precedence. That is, an application.properties file in a /config subdirectory will override the same properties set in an application.properties file in the application’s classpath.  
同样的，这个列表按照优先级排序。也就是说/config子目录里的application.properties会覆盖应用程序Classpath里的application.properties中的相同属性。

Also, I’ve found that if you have both application.properties and application.yml side by side at the same level of precedence, properties in application.yml will override those in application.properties.  
此外，如果你在同一优先级位置同时有application.properties和application.yml，那么application.yml里的属性会覆盖application.properties里的。

Disabling an ascii-art banner is just a small example of how to use properties. Let’s look at a few more common ways to tweak the auto-configured beans.  
禁用ascii-art Banner只是个小例子，再让我们看几个例子，如何通过常用途径微调自动配置的Bean。

### 3.2.1 Fine-tuning auto-configuration
### 3.2.1 自动配置微调

As I said, there are well over 300 properties that you can set to tweak and adjust the beans in a Spring Boot application. Appendix C gives an exhaustive list of these properties, but it’d be impossible to go over each and every one of them here. Instead, let’s examine a few of the more commonly useful properties exposed by Spring Boot.  
如我所说，有超过300个属性可以用来微调Spring Boot应用程序里的Bean。附录C有一个详尽的列表，此处无法逐一描述它们的细节，因此我们就通过几个例子来了解一些Spring Boot暴露的有用的属性。

#### DISABLING TEMPLATE CACHING
#### 禁用模板缓存

If you’ve been tinkering around much with the reading-list application, you may have noticed that changes to any of the Thymeleaf templates aren’t applied unless you restart the application. That’s because Thymeleaf templates are cached by default. This improves application performance because you only compile the templates once, but it’s difficult to make changes on the fly during development.  
如果你的阅读列表应用程序已经经过几番修改，一定已经注意到了，除非重启应用程序，否则对Thymeleaf模板的变更是不会生效的。这是因为Thymeleaf模板默认是被缓存的。这有助于改善应用程序的性能，因为模板只需编译一次，但在开发过程中就不能实时看到变更的效果。

You can disable Thymeleaf template caching by setting spring.thymeleaf.cache to false. You can do this when you run the application from the command line by setting it as a command-line argument:  
将`spring.thymeleaf.cache`设置为`false`就能禁用Thymeleaf模板缓存。在命令行里运行应用程序时，将其设置为命令行参数即可：

```
$ java -jar readinglist-0.0.1-SNAPSHOT.jar --spring.thymeleaf.cache=false
```

Or, if you’d rather have caching turned off every time you run the application, you might create an application.yml file with the following lines:  
或者，如果你希望每次运行时都禁用缓存，可以创建一个application.yml，包含以下内容：

```
spring:
  thymeleaf:
    cache: false
```

You’ll want to make sure that this application.yml file doesn’t follow the application into production, or else your production application won’t realize the performance benefits of template caching.  
你一定要确保这个文件不会被发布到生产环境，否则生产环境里的应用程序就无法享受模板缓存带来的福利了。

As a developer, you may find it convenient to have template caching turned off all of the time while you make changes to the templates. In that case, you can turn off Thymeleaf caching via an environment variable:  
作为开发者，在修改模板时始终关闭缓存实在太方便了。为此，可以通过环境变量来禁用Thymeleaf缓存：

```
$ export spring_thymeleaf_cache=false
```

Even though we’re using Thymeleaf for our application’s views, template caching can be turned off for Spring Boot’s other supported template options by setting these properties:  
我们使用Thymeleaf作为应用程序的视图，Spring Boot支持的其他模板也能关闭模板缓存，设置这些属性就好了：

* spring.freemarker.cache (Freemarker)
* spring.groovy.template.cache (Groovy templates)
* spring.velocity.cache (Velocity)

* spring.freemarker.cache（Freemarker）
* spring.groovy.template.cache（Groovy模板）
* spring.velocity.cache（Velocity）

By default, all of these properties are true, meaning that the templates are cached. Setting them to false disables caching.  
默认情况下，这些属性都为`true`，也就是开启缓存，将它们设置为`false`即可禁用缓存。

#### CONFIGURING THE EMBEDDED SERVER
#### 配置嵌入式服务器

When you run a Spring Boot application from the command line (or via Spring Tool Suite), the application starts an embedded server (Tomcat, by default) listening on port 8080. This is fine for most cases, but it can become problematic if you find yourself needing to run multiple applications simultaneously. If all of the applications try to start a Tomcat server on the same port, there’ll be port collisions starting with the second application.  
从命令行（或者Spring Tool Suite）运行Spring Boot应用程序时，应用程序会启动一个嵌入式的服务器（默认是Tomcat），监听8080端口。大部分情况下这挺好的，但如果你要同时运行多个应用程序可能就会有问题了。要是所有应用程序都试着让Tomcat服务器监听同一个端口，在启动第二个应用程序时就会有冲突。

If, for any reason, you’d rather the server listen on a different port, then all you need to do is set the server.port property. If this is a one-time change, it’s easy enough to do this as a command-line argument:  
出于各种原因，你想让服务器监听不同的端口，你所要做的就是设置`server.port`属性。要是只改一次，可以用命令行参数：

```
$ java -jar readinglist-0.0.1-SNAPSHOT.jar --server.port=8000
```

But if you want the port change to be more permanent, you could set server.port in one of the other supported locations. For instance, you might set it in an application.yml file at the root of the application’s classpath:  
但如果希望端口变更时间更长一点，可以在其他支持的配置位置上设置`server.port`。例如，把它放在应用程序Classpath根目录的application.yml文件里：

```
server:
  port: 8000
```

Aside from adjusting the server’s port, you might also need to enable the server to serve securely over HTTPS. The first thing you’ll need to do is create a keystore using the JDK’s keytool utility:  
除了服务器的端口，你还可能希望服务器能提供HTTPS服务。为此，第一步就是用JDK的`keytool`工具来创建一个密钥存储（keystore）：

```
$ keytool -keystore mykeys.jks -genkey -alias tomcat -keyalg RSA
```

You’ll be asked several questions about your name and organization, most of which are irrelevant. But when asked for a password, be sure to remember what you choose. For the sake of this example, I chose “letmein” as the password.  
该工具会询问几个与名字和组织相关的问题，大部分都无关紧要。但在询问密码时，一定要记住你的选择。在本例中，我选择“letmein”作为密码。

Now you just need to set a few properties to enable HTTPS in the embedded server. You could specify them all at the command line, but that would be terribly inconvenient. Instead, you’ll probably set them in application.properties or application.yml. In application.yml, they might look like this:  
现在只需要设置几个属性就能开启嵌入式服务器的HTTPS服务了。可以把它们都配置在命令行里，但这样太不方便了，可以把它们放在application.properties或application.yml里。在application.yml中，它们可能是这样的：

```
server:
  port: 8443
  ssl:
    key-store: file:///path/to/mykeys.jks
    key-store-password: letmein
    key-password: letmein
```

Here the server.port property is being set to 8443, a common choice for development HTTPS servers. The server.ssl.key-store property should be set to the path where the keystore file was created. Here it’s shown with a file:// URL to load it from the filesystem, but if you package it within the application JAR file, you should use a classpath: URL to reference it. And both the server.ssl.key-store-password and server.ssl.key-password properties are set to the password that was given when creating the keystore.  
此处的`server.port`设置为8443，开发环境的HTTPS服务器大多会选这个端口。`server.ssl.key-store`属性指向密钥存储文件的存放路径，这里用了一个file://开头的URL，从文件系统里加载该文件。你也可以把它打在应用程序的JAR文件里，用classpath: URL来引用它。`server.ssl.key-store-password`和`server.ssl.key-password`设置为创建该文件时给定的密码。

With these properties in place, your application should be listening for HTTPS requests on port 8443. (Depending on which browser you’re using, you may encounter a warning about the server not being able to verify its identity. This is nothing to worry about when serving from localhost during development.)  
有了这些属性，应用程序就能在8443端口上监听HTTPS请求了。（根据你所用的浏览器，可能会出现警告框提示该服务器无法验证其身份。在开发时，访问的是localhost，这没什么好担心的。）

#### CONFIGURING LOGGING
Most applications provide some form of logging. And even if your application doesn’t log anything directly, the libraries that your application uses will certainly log their activity.

By default, Spring Boot configures logging via Logback (http://logback.qos.ch) to log to the console at INFO level. You’ve probably already seen plenty of INFO-level logging as you’ve run the application and other examples.

>__Swapping out Logback for another logging implementation__
>Generally speaking, you should never need to switch logging implementations; Log- back should suit you fine. However, if you decide that you’d rather use Log4j or Log4j2, you’ll need to change your dependencies to include the appropriate starter for the logging implementation you want to use and to exclude Logback.

>For Maven builds, you can exclude Logback by excluding the default logging starter transitively resolved by the root starter dependency:

>```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter</artifactId>
  <exclusions>
    <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

>In Gradle, it’s easiest to place the exclusion under the configurations section:

>```
configurations {
  all*.exclude group:'org.springframework.boot',
               module:'spring-boot-starter-logging'
}
```

>With the default logging starter excluded, you can now include the starter for the log- ging implementation you’d rather use. With a Maven build you can add Log4j like this:

>```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-log4j</artifactId>
</dependency>
```

>In a Gradle build you can add Log4j like this:
>```
compile("org.springframework.boot:spring-boot-starter-log4j")
```

>If you’d rather use Log4j2, change the artifact from “spring-boot-starter-log4j” to “spring-boot-starter-log4j2”.

For full control over the logging configuration, you can create a logback.xml file at the root of the classpath (in src/main/resources). Here’s an example of a simple log- back.xml file you might use:

```
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>
        %d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
      </pattern>
    </encoder>
  </appender>

  <logger name="root" level="INFO"/>

  <root level="INFO">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```

Aside from the pattern used for logging, this Logback configuration is more or less equivalent to the default you’ll get if you have no logback.xml file. But by editing log- back.xml you can gain full control over your application’s log files. The specifics of what can go into logback.xml are outside the scope of this book, so refer to Logback’s documentation for more information.

Even so, the most common changes you’ll make to a logging configuration are to change the logging levels and perhaps to specify a file where the logs should be writ- ten. With Spring Boot configuration properties, you can make those changes without having to create a logback.xml file.

To set the logging levels, you create properties that are prefixed with logging.level, followed by the name of the logger for which you want to set the logging level. For instance, suppose you’d like to set the root logging level to WARN, but log Spring Security logs at DEBUG level. The following entries in application.yml will take care of it for you:

```
logging:
  level:
    root: WARN
    org:
      springframework:
        security: DEBUG
```

Optionally, you can collapse the Spring Security package name to a single line:

```
logging:
  level:
    root: WARN
    org.springframework.security: DEBUG
```

Now suppose that you want to write the log entries to a file named BookWorm.log at /var/logs/. The logging.path and logging.file properties can help with that:

```
logging:
  path: /var/logs/
  file: BookWorm.log
  level:
    root: WARN
    org:
      springframework:
        security: DEBUG
```

Assuming that the application has write permissions to /var/logs/, the log entries will be written to /var/logs/BookWorm.log. By default, the log files will rotate once they hit 10 megabytes in size.

Similarly, all of these properties can be set in application.properties like this:

```
logging.path=/var/logs/
logging.file=BookWorm.log
logging.level.root=WARN
logging.level.root.org.springframework.security=DEBUG
```

If you still need full control of the logging configuration, but would rather name the Logback configuration file something other than logback.xml, you can specify a cus- tom name by setting the logging.config property:

```
logging:
  config:
    classpath:logging-config.xml
```

Although you usually won’t need to change the configuration file’s name, it can come in handy if you want to use two different logging configurations for different runtime profiles (see section 3.2.3).
