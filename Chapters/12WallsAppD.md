# 附录D Spring Boot依赖

无论你在构建项目时用的是Maven、Gradle还是Spring Boot CLI，Spring Boot都为Spring应用程序常用的很多库提供了依赖管理支持功能。表D-1列出了Spring Boot 1.3.0版本支持的所有库依赖。

在很多情况下，这些依赖都会通过某个Spring Boot起步依赖自动添加到项目和Classpath里（如附录A所述）。但是，如果你正在使用的起步依赖没有覆盖到的某个库，而你需要使用这个库，那就得在Maven或Gradle的构建说明里显式地声明这个依赖。

举例来说，如果你的项目需要引入H2嵌入式数据库，那么你需要在Gradle里加入如下声明：
```
compile("com.h2database:h2")
```
在Maven里可以添加类似的声明：
```
<dependency>
  <groupId>com.h2database</groupId>
  <version>h2</version>
</dependency>
```
请注意，在这两种情况下，都不需要指定版本号，Spring Boot的依赖管理会替你处理这个问题的。但是，如果你想覆盖Spring Boot选择的版本，你也可以显式地提供一个版本号。

如果你在使用Spring Boot CLI运行应用程序，可以在Groovy里像这样使用`@Grab`注解：
```
@Grab("h2")
```
在使用`@Grab`注解引入表D-1里的库时，你只需要指定Artifact ID就可以了，Spring Boot扩展了`@Grab`，让它可以推测出Group ID和版本号。

__表D-1 Spring Boot支持的库依赖__

> P233~241表格，表格内容扫原书，表头翻译如下：

| Group ID | Artifact ID | 版本号 |
|----------|-------------|-------|
