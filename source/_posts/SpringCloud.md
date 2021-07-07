---
title: SpringCloud
date: 2021-05-26 18:12:04
tags:
---

项目地址：https://github.com/Hu414706657/demo-spring-cloud


## 服务注册中心eureka
新建 **`spring-cloud-eureka`** 用来做服务注册与发现
- Maven依赖
    ```java 
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
    </dependency>
    ```
- application.yml
```text
spring:
  application:
    name: spring-cloud-eureka

server:
  port: 8761

eureka:
  instance:
    hostname: localhost
  client:
    registerWithEureka: false
    fetchRegistry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/

```
[配置说明](https://github.com/Hu414706657/demo-spring-cloud/blob/master/spring-cloud-eureka/src/main/resources/%E9%85%8D%E7%BD%AE%E8%AF%B4%E6%98%8E)

- 启动类添加注解 **`@EnableEurekaServer`**

    启动项目，访问http://127.0.0.1:8761/,如图所示
    ![](https://414706657.oss-cn-shenzhen.aliyuncs.com/spring_cloud/spring-cloud_1.png)

## 服务提供者
新建项目 **`spring-cloud-service-provider`**
- Maven依赖
    ```java
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    ```
- application.yml
```text
spring:
  application:
    name: SPRING-CLOUD-SERVICE-PROVIDER
server:
  port: 8762
eureka:
  instance:
#    instance-id: testid
#    appname: test1
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
    register-with-eureka: true
    fetch-registry: true
hystrix:
  metrics:
    enabled: true
    polling-interval-ms: 2000
```
- 启动类添加 **`@EnableHystrix`** 注解

    创建 **`Controller`** 并对外提供hi接口，启动项目如图所示服务注册成功
    ![](https://414706657.oss-cn-shenzhen.aliyuncs.com/spring_cloud/spring-cloud_2.png)
    访问：http://localhost:8762/hi?message=test 能正常返回数据

## 服务消费者
新建项目 **`spring-cloud-service-consumer-feign`**，feign是默认集成了ribbon

- Maven依赖
```java
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
    <version>2.2.8.RELEASE</version>
</dependency>
```

- application.yml
```text
spring:
  application:
    name: spring-cloud-consumer-feign

server:
  port: 8765

eureka:
  instance:
  #    instance-id: testid
  #    appname: test1
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
```

- 启动类添加 **`@EnableDiscoveryClient`** **`@EnableFeignClients`** 注解

- 接口类：**`DemoService`**
```java
package com.example.spring.cloud.consumer.feign.service;


import com.example.spring.cloud.consumer.feign.controller.fallback.ErrorBallback;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;

@FeignClient(value = "SPRING-CLOUD-SERVICE-PROVIDER")
public interface DemoService {

    @RequestMapping(value = "hi", method = RequestMethod.GET)
    public String sayHi(@RequestParam(value = "message") String message);

}
```
**`@FeignClient`** 为服务提供者项目名称(配置文件：spring.application.name)

- 启动项目，如图所示
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/spring_cloud/spring-cloud_3.png)
访问：http://localhost:8765/hi?message=test 提示端口为8762，说明服务调用成功