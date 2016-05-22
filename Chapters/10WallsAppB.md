# appendix B Spring Boot starters
# 附录B Spring Boot起步依赖

Spring Boot starter dependencies greatly simplify the dependencies section of your project’s build specification by aggregating commonly used dependencies under more coarse-grained dependencies. Your build will transitively resolve the dependencies that are declared in the starter dependency.  
Spring Boot起步依赖在很大程度上简化了项目构建说明中的依赖配置，它将常用的依赖用更粗粒度的依赖聚合到了一起。你的构建项目会传递解析到起步依赖中声明的其他依赖。
Not only do starter dependencies keep the dependencies section of the build smaller, they are typically organized by the type of functionality they bring to an application. For example, rather than specify specific libraries required for validation (such as Hibernate Validator and Tomcat’s embedded expression language), you can simply add the spring-boot-starter-validation starter as a dependency.  
起步依赖不仅能让构建说明中的依赖配置更间断，还能根据它们为应用程序带来的功能组织到一起。例如，与其指定用于验证的特定库（比如Hibernate Validator和Tomcat的内嵌表达式语言），还不如简单地添加`spring-boot-starter-validation`起步依赖。Table B.1 lists all of Spring Boot’s starter dependencies along with the depen- dencies that they transitively declare.  
表B.1列出了Spring Boot的所有起步依赖，还有它们传递声明的依赖。

__Table B.1 Spring Boot starters__  
__表B.1 Spring Boot起步依赖__

| Starter (Group ID: org.springframework.boot) | Transitively depends on |
|----------------------------------------------|-------------------------|

| 起步依赖(Group ID：org.springframework.boot） | 传递依赖 |
|---------------------------------------------|--------|


表格内容不做翻译。
