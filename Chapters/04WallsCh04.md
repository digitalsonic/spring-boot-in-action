# Testing with Spring Boot
# Spring Boot测试

__This chapter covers__  
__本章内容涉及__

* Integration testing
* Testing apps in a server
* Spring Boot’s test utilities

It’s been said that if you don’t know where you’re going, any road will get you there. But with software development, if you don’t know where you’re going, you’ll likely end up with a buggy application that nobody can use.

The best way to know for sure where you’re going when writing applications is to write tests that assert the desired behavior of an application. If those tests fail, you know you have some work to do. If they pass, then you’ve arrived (at least until you think of some more tests that you can write).

Whether you write tests first or after the code has already been written, it’s important that you write tests to not only verify the accuracy of your code, but to also to make sure it does everything you expect it to. Tests are also a great safeguard to make sure that things don’t break as your application continues to evolve.

When it comes to writing unit tests, Spring is generally out of the picture. Loose coupling and interface-driven design, which Spring encourages, makes it really easy to write unit tests. But Spring isn’t necessarily involved in those unit tests.

Integration tests, on the other hand, require some help from Spring. If Spring is responsible for configuring and wiring up the components in your production application, then Spring should also be responsible for configuring and wiring up those components in your tests.

Spring’s SpringJUnit4ClassRunner helps load a Spring application context in JUnit-based application tests. Spring Boot builds on Spring’s integration testing support by enabling auto-configuration and web server startup when testing Spring Boot applications. It also offers a handful of useful testing utilities.

In this chapter, we’ll look at all of the ways that Spring Boot supports integration testing. We’ll start by looking at how to test with a fully Spring Boot-enabled application context.

## 4.1 Integration testing auto-configuration

At the core of everything that the Spring Framework does, its most essential task is to wire together all of the components that make up an application. It does this by read- ing a wiring specification (whether it be XML, Java-based, Groovy-based, or otherwise), instantiating beans in an application context, and injecting beans into other beans that depend on them.

When integration testing a Spring application, it’s important to let Spring wire up the beans that are the target of the test the same way it wires up those beans when the application is running in production. Sure, you might be able to manually instantiate the components and inject them into each other, but for any substantially big application, that can be an arduous task. Moreover, Spring offers additional facilities such as component-scanning, autowiring, and declarative aspects such as caching, transactions, and security. Given all that would be required to recreate what Spring does, it’s generally best to let Spring do the heavy lifting, even in an integration test.

Spring has offered excellent support for integration testing since version 1.1.1. Since Spring 2.5, integration testing support has been offered in the form of SpringJUnit4ClassRunner, a JUnit class runner that loads a Spring application context for use in a JUnit test and enables autowiring of beans into the test class.

For example, consider the following listing, which shows a very basic Spring integration test.
