# Applying Grails in Spring Boot
# 在Spring Boot中使用Grails

__This chapter covers__
__本章内容涉及__

* Persisting data with GORM
* Defining GSP views
* An introduction to Grails 3 and Spring Boot
* 使用GORM持久化数据
* 定义GSP视图
* Grails 3和Spring Boot入门

When I was growing up, there was a series of television advertisements involving two people, one enjoying a chocolate bar and another eating peanut butter out of a jar. By way of some sort of comedic mishap, the two would collide, resulting in the peanut butter and chocolate getting mixed.  
我小时候，有一系列的电视广告，当中有两个人，一个在吃巧克力条，另一个在吃罐里的花生酱。经由一些富有喜剧效果的小事故，两个人撞到一起，最后花生酱和巧克力结合到了一起。

One would proclaim, “You got your chocolate in my peanut butter!” The other would respond, “You got peanut butter on my chocolate!”  
一个人会说，“你把巧克力弄到我的花生酱里了！”另一个回答，“是你把花生酱弄到我的巧克力上了！”

After initially being angry with their circumstances, the two would conclude that the combination of peanut butter and chocolate is a good thing. Then a voice-over would suggest that the viewer should eat a Reese’s Peanut Butter Cup.  
在一开始的尴尬气氛后，两个人都认同花生酱和巧克力结合在一起是件好事，接着旁白会建议观众试试Reese的Peanut Butter Cup。

From the moment that Spring Boot was announced, I’ve been frequently asked how to choose between Spring Boot and Grails. Both are built upon the Spring Framework and both help ease application development. Indeed, they’re very much like peanut butter and chocolate. Both are great, but the choice is largely a personal one.  
在Spring Boot刚发布时，经常有人问我在Spring Boot和Grails之间该如何选择。两者都构建于Spring Framework之上，都旨在简化应用程序的开发。实际上，它们就像花生酱和巧克力。两个都很好，这个选择取决于个人爱好。

As it turns out, there’s no reason to choose one or the other. Just like the chocolate vs. peanut butter debate, Spring Boot and Grails are two great choices that work great together.  
后来发现并没理由从中选一个出来，就像之前巧克力和花生酱的争论一样，Spring Boot和Grails两个都很好，完全可以结合到一起。

In this chapter, we’re going to look at the connection between Grails and Spring Boot. We’ll start by looking at a few Grails features like GORM and Groovy Server Pages (GSP) that are available in Spring Boot. Then we’ll flip it around and see how Grails 3 has been reinvented by being built upon Spring Boot.  
在本章中，我们会看到Grails和Spring Boot之前的联系。首先是在Spring Boot里能用到的GORM和Groovy Server Pages（GSP）这样的Grails特性，接下来再看看Grails 3是如何基于Spring Boot被重写的。

## 6.1 Using GORM for data persistence
## 6.1 使用GORM进行数据持久化

Probably one of the most intriguing pieces of Grails is GORM (Grails object-relational mapping). GORM makes database work as simple as declaring the entities that will be persisted. For example, listing 6.1 shows how the Book entity from the reading-list example could be written in Groovy as a GORM entity.  
Grails里最让人着迷的恐怕就是GORM了（Grails的对象关系映射）。GORM能简化数据库相关的工作，就和声明要持久化的实体一样容易。例如，代码6.1演示了阅读列表里的`Book`该如何用Groovy写成GORM实体。

__Listing 6.1 A GORM Book entity__  
__代码6.1 GORM `Book`实体__

```
package readinglist

import grails.persistence.*

@Entity
class Book {

  Reader reader
  String isbn
  String title
  String author
  String description

}
```

This is a GORM entity  
这是一个GORM实体

Just like its Java equivalent, this Book class has a handful of properties that describe a book. Unlike the Java version, however, it’s not littered with semicolons, public or private modifiers, setter and getter methods, or any of the other noise that’s common in Java. But what makes it a GORM entity is that it’s annotated with the @Entity annotation from Grails. This simple entity does a lot, including mapping the object to the database and enabling Book with persistence methods through which it can be saved and retrieved.  
就和`Book`的Java版本一样，这个类里有很多描述图书的属性。但又与Java版本不一样，没有分号、`public`或`private`修饰符、setter和getter方法或其他Java中常见的代码噪声。是Grails的`@Entity`注解让这个类变成了GORM实例。这个简单的实体可干了不少事，包括将对象映射到数据库，为`Book`添加持久化方法，通过这些方法可以存取图书。

To use GORM with a Spring Boot project, all you must do is add the GORM dependency to your build. In Maven, the <dependency> looks like this:  
要在Spring Boot项目里使用GORM，必须在项目里添加GORM依赖。在Maven中，`<dependency>`看起来是这样的：

```
<dependency>
  <groupId>org.grails</groupId>
  <artifactId>gorm-hibernate4-spring-boot</artifactId>
  <version>1.1.0.RELEASE</version>
</dependency>
```

The same dependency can be expressed in a Gradle build like this:  
一样的依赖，在Gradle里是这样的：

```
compile("org.grails:gorm-hibernate4-spring-boot:1.1.0.RELEASE")
```

This library carries some Spring Boot auto-configuration with it that will automatically configure all of the necessary beans to support working with GORM. All you need to do is start writing the code.  
这个库自带了一些Spring Boot自动配置，会自动配置所有支持GORM所需的Bean。你只管写代码就好了。

> __Another GORM option for Spring Boot__  
__GORM在Spring Boot里的另一个选择__

> As its name suggests, the gorm-hibernate4-spring-boot dependency enables GORM for data persistence via Hibernate. For many projects, this will be fine. If, however, you’re interested in working with the MongoDB document database, you’ll be pleased to know that GORM for MongoDB is also available for Spring Boot.  
正如其名，`gorm-hibernate4-spring-boot`是通过Hibernate开启GORM数据持久化的。对很多项目而言，这挺好的。但如果你想用MongoDB，那会Spring Boot里的MongoDB GORM支持很感兴趣。

> The Maven dependency looks like this:  
它的Maven依赖是这样的：

>
```
<dependency>
  <groupId>org.grails</groupId>
  <artifactId>gorm-mongodb-spring-boot</artifactId>
  <version>1.1.0.RELEASE</version>
</dependency>
```

> Likewise, the Gradle dependency is as follows:  
同样的，下面是Gradle依赖：
```
compile("org.grails:gorm-mongodb-spring-boot:1.1.0.RELEASE")
```

Due to the nature of how GORM works, it requires that at least the entity class be written in Groovy. We’ve already written the Book entity in listing 6.1. As for the Reader entity, it’s shown in the following listing.  
鉴于GORM的本质，它要求实体类必须用Groovy来编写。我们已经在代码6.1里写了一个`Book`实体，下面再写一个`Reader`实体。

__Listing 6.2 A GORM Reader entity__  
__代码6.2 GORM `Reader`实体__

```
package readinglist

import grails.persistence.*

import org.springframework.security.core.GrantedAuthority
import
    org.springframework.security.core.authority.SimpleGrantedAuthority
import org.springframework.security.core.userdetails.UserDetails

@Entity
class Reader implements UserDetails {

  String username
  String fullname
  String password

  Collection<? extends GrantedAuthority> getAuthorities() {
    Arrays.asList(new SimpleGrantedAuthority("READER"))
  }

  boolean isAccountNonExpired() {
    true
  }

  boolean isAccountNonLocked() {
    true
  }

  boolean isCredentialsNonExpired() {
    true
  }

  boolean isEnabled() {
    true
  }
}
```

This is an entity  
这是一个实体

Implement UserDetails  
实现了`UserDetails`

Now that we’ve written the two GORM entities for the reading-list application, we’ll need to rewrite the rest of the app to use them. Because working with Groovy is such a pleasant experience (and very Grails-like), we’ll continue writing the other classes in Groovy as well.  
现在，我们的阅读列表应用程序里有了两个GORM实体，我们需要重写剩下的应用程序来使用这两个实体。因为使用Groovy是如此的令人愉悦（和Grails很像），所以其他类我们也会用Groovy来编写。

First up is ReadingListController, as shown next.  
首先是`ReadingListController`，就像下面这样。

__Listing 6.3 A Groovy reading-list controller__  
__代码6.3 Groovy的`ReadingListController`__

```
package readinglist

import org.springframework.beans.factory.annotation.Autowired
import
    org.springframework.boot.context.properties.ConfigurationProperties
import org.springframework.http.HttpStatus
import org.springframework.stereotype.Controller
import org.springframework.ui.Model
import org.springframework.web.bind.annotation.ExceptionHandler
import org.springframework.web.bind.annotation.RequestMapping
import org.springframework.web.bind.annotation.RequestMethod
import org.springframework.web.bind.annotation.ResponseStatus

@Controller
@RequestMapping("/")
@ConfigurationProperties("amazon")
class ReadingListController {
  @Autowired
  AmazonProperties amazonProperties

  @ExceptionHandler(value=RuntimeException.class)
  @ResponseStatus(value=HttpStatus.BANDWIDTH_LIMIT_EXCEEDED)
  def error() {
    "error"
  }

  @RequestMapping(method=RequestMethod.GET)
  def readersBooks(Reader reader, Model model) {
    List<Book> readingList = Book.findAllByReader(reader)
    model.addAttribute("reader", reader)
    if (readingList) {
      model.addAttribute("books", readingList)
      model.addAttribute("amazonID", amazonProperties.getAssociateId())
    }
    "readingList"
  }

  @RequestMapping(method=RequestMethod.POST)
  def addToReadingList(Reader reader, Book book) {
    Book.withTransaction {
      book.setReader(reader)
      book.save()
    }
    "redirect:/"
  }

}  
```

Find all reader books  
查找读者的全部图书

Save a book  
保存一本书


The most obvious difference between this version of ReadingListController and the one from chapter 3 is that it’s written in Groovy and lacks much of the code noise from Java. But the most significant difference is that it doesn’t work with an injected ReadingListRepository anymore. Instead, it works directly with the Book type for persistence.

In the readersBooks() method, it calls the static findAllByReader() method on Book to fetch all books for the given reader. Although we didn’t write a findAllByReader() method in listing 6.1, this will work because GORM will implement it for us.

Likewise, the addToReadingList() method uses the static withTransaction() and the instance save() methods, both provided by GORM, to save a Book to the database.

And all we had to do was declare a few properties and annotate Book with @Entity. A pretty good payoff, if you ask me.

A similar change must be made to SecurityConfig to fetch a Reader via GORM rather than using ReadingListRepository. The following listing shows the new Groovy SecurityConfig.

__Listing 6.4 SecurityConfig in Groovy__

```
package readinglist

import org.springframework.context.annotation.Configuration
import org.springframework.security.config.annotation.authentication.
                                  builders.AuthenticationManagerBuilder
import org.springframework.security.config.annotation.web.
                                                  builders.HttpSecurity
import org.springframework.security.config.annotation.web.
                             configuration.WebSecurityConfigurerAdapter
import org.springframework.security.core.userdetails.UserDetailsService

@Configuration
class SecurityConfig extends WebSecurityConfigurerAdapter {

  void configure(HttpSecurity http) throws Exception {
    http
      .authorizeRequests()
      .antMatchers("/").access("hasRole('READER')")
      .antMatchers("/**").permitAll()
      .and()
      .formLogin()
      .loginPage("/login")
      .failureUrl("/login?error=true")
  }

  void configure(AuthenticationManagerBuilder auth) throws Exception {
    auth
      .userDetailsService(
        { username -> Reader.findByUsername(username) }
        as UserDetailsService)
  }

}
```

Find a reader by username

Aside from being rewritten in Groovy, the most significant change in SecurityConfig is the second configure() method. As you can see, it uses a closure (as the implementation of UserDetailsService) that looks up a Reader by calling the static findByUsername() method, which is provided by GORM.

You may be wondering what becomes of ReadingListRepository in this GORMenabled application. With GORM handling all of the persistence for us, ReadingListRepository is no longer needed. Neither are any of its implementations. I think you’ll agree that less code is a good thing.

As for the remaining code in the application, it should also be rewritten in Groovy to match the classes we’ve changed thus far. But none of it deals with GORM and is therefore out of scope for this chapter. The complete Groovy application is available in the example code download.

At this point, you can fire up the reading-list application using any of the ways we’ve already discussed for running Spring Boot applications. Once it starts, the application should work as it always has. Only you and I know that the persistence mechanism has been changed.

In addition to GORM, Grails apps usually use Groovy Server Pages to render model data as HTML served to the browser. The Grails-ification of our application continues in the next section, where we’ll replace the Thymeleaf templates with equivalent GSP.
