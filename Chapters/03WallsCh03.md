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

Grant READER privilege

Do not expire, lock, or disable

As you can see, Reader is annotated with @Entity to make it a JPA entity. In addition, its username field is annotated with @Id to designate it as the entity’s ID. This seemed like a natural choice, as the username should uniquely identify the Reader.
