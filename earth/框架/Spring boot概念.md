# 概念
* Spring Boot实现了自动配置，降低了项目搭建的复杂度。

* 众所周知Spring框架需要进行大量的配置，Spring Boot引入自动配置的概念，让项目设置变得很容易。Spring Boot本身并不提供Spring框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于Spring框架的应用程序。也就是说，它并不是用来替代Spring的解决方案，而是和Spring框架紧密结合用于提升Spring开发者体验的工具。同时它集成了大量常用的第三方库配置(例如Jackson, JDBC, Mongo, Redis, Mail等等)，Spring Boot应用中这些第三方库几乎可以零配置的开箱即用(out-of-the-box)，大部分的Spring Boot应用都只需要非常少量的配置代码，开发者能够更加专注于业务逻辑。

* applicationContext.xml文件是Spring框架需要的一个全局配置文件, Spring Boot不再需要配置applicationContext.xml, 如果还需要类似的配置文件, 可以通过 @ImportResource 注解来引入xml文件. 文件名当然可以自定义.

* web.xml文件是Java Web项目需要的配置文件, 配置servlet相关的信息. servlet3.0支持了注解配置, 不再需要web.xml, Spring Boot利用这个特性, web.xml也省掉了.

* 可以使用application.propertie里配置spring.profiles.active来分区环境.


# Spring Boot核心模块
## spring-boot-starter-web
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
spring-boot-starter-web是web支持模块, 这个模块包含了很多web请求相关的jar包, 基本可以涵盖一个web应用, 即spring mvc应用所需的jar包了.

spring-boot-starter-web包含了核心的: 
* spring-boot-starter
* jackson
* spring-core
* spring-mvc
* spring-boot-starter-tomcat


而spring-boot-starter-tomcat包含了一个嵌入的tomcat应用, 包括: 
* core
* el
* logging
* websocket

## spring-boot-starter-test
测试模块，包括 JUnit、Hamcrest、Mockito。


