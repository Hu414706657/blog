---
title: SpringBoot
date: 2019-07-22 14:48:07
tags:
    - spring
---

# 概述

随着动态语言的流行 (Ruby、Groovy、Scala、Node.js)，Java 的开发显得格外的笨重：繁多的配置、低下的开发效率、复杂的部署流程以及第三方技术集成难度大。

在上述环境下，Spring Boot 应运而生。它使用“习惯优于配置”（项目中存在大量的配置，此外还内置了一个习惯性的配置，让你无需手动进行配置）的理念让你的项目快速的运行起来。使用 Spring Boot 很容易创建一个独立运行（运行 Jar，内嵌 Servlet 容器）准生产级别的基于 Spring 框架的项目，使用 Spring Boot 你可以不用或者只需很少的 Spring 配置。


## 优点
- 快速构建项目
- 对主流开发框架的无配置集成
- 项目可独立运行，无需外部依赖 Servlet 容器
- 提供运行时的应用监控
- 极大地提高了开发、部署效率
- 与云计算的天然集成

## 缺点
- 版本迭代速度很快，一些模块改动很大
- 由于不用自己做配置，报错时很难定位
- 网上现成的解决方案比较少

# pom.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example.demo</groupId>
    <artifactId>spring-boot-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-boot-demo</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```
- parent：继承了 Spring Boot 的 Parent，表示我们是一个 Spring Boot 工程
- `spring-boot-starter-web`：包含了 `spring-boot-starter` 还自动帮我们开启了 Web 支持

# application.properties配置文件
application.properties(application.yml)为boot项目中的一个系统自带的全局属性配置文件. 提供默认属性重写的作用. 可包含重写系统tomcat,spring,springmvc,mybatis等诸多默认配置属性:

#全局配置文件: 重写视图解析器的资源地址.
#页面默认前缀目录
spring.mvc.view.prefix=/WEB-INF/jsp/
#响应页面默认后缀
spring.mvc.view.suffix=.jsp
#静态资源目录配置,
spring.mvc.static-path-pattern=/static/**

#tomcat服务器的配置:
server.port=8081
server.servlet.context-path=/sb2

#默认支持的日志记录:
#logging.config=classpath:logback.xml 加载单独的日志配置文件.
logging.file=d:/test/log.log
logging.level.org.springframework.web=DEBUG

#提供jdbc的基本配置:
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/c01?useUnicode=true&characterEncoding=utf-8
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.type=org.apache.commons.dbcp.BasicDataSource
#提供mybatis的属性配置: 扫描.
mybatis.mapper-locations=classpath:mapper/*_mapper.xml

# springboot中的核心启动主函数
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringBootDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootDemoApplication.class, args);
    }

}
```
- 该主函数: 主要启动springboot框架.用于加载容器和诸多默认组件.
- 用到核心注解: `@SpringBootApplication`, 作用:用于标识声明一个springboot框架容器.


# springBoot中常用的starter的组件

- spring-boot-starter-parent //boot项目继承的父项目模块.
- spring-boot-starter-web //boot项目集成web开发模块.
- spring-boot-starter-tomcat //boot项目集成tomcat内嵌服务器.
- spring-boot-starter-test //boot项目集成测试模块.
- mybatis-spring-boot-starter //boot项目集成mybatis框架.
- spring-boot-starter-jdbc //boot项目底层集成jdbc实现数据库操作支持.

# springBoot自动配置原理
Spring Boot有一个全局配置文件：application.properties或application.yml。我们的各种属性都可以在这个文件中进行配置，最常配置的比如：server.port、logging.level.* 等等

SpringBoot 自动配置主要通过 `@EnableAutoConfiguration`, `@Conditional`, `@EnableConfigurationProperties` 或者 `@ConfigurationProperties` 等几个注解来进行自动配置完成的。

- `@EnableAutoConfiguration` 开启自动配置，主要作用就是调用 Spring-Core 包里的 loadFactoryNames()，将 autoconfig 包里的已经写好的自动配置加载进来。

- `@Conditional` 条件注解，通过判断类路径下有没有相应配置的 jar 包来确定是否加载和自动配置这个类。

- `@EnableConfigurationProperties` 的作用就是，给自动配置提供具体的配置参数，只需要写在 application.properties 中，就可以通过映射写入配置类的 POJO 属性中。

**工作原理剖**

Spring Boot关于自动配置的源码在spring-boot-autoconfigure-x.x.x.x.jar中,当我们程序去依赖了这些组件的 jar 包后，启动 SpringBoot应用，于是自动加载开始了。