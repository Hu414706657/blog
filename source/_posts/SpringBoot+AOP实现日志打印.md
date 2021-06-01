---
title: SpringBoot+AOP实现日志打印
date: 2021-05-28 18:48:43
tags:
---

## 项目地址：https://github.com/Hu414706657/spring-boot-aop-demo

### 主要依赖添加

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

### 项目结构
```text
    .
    ├── aspect         # 日志打印切面类
    ├── controller     # 模拟controller层
    ├── entity         # Post请求body参数对象
    ├── filte          # 过滤器
    ├── helper         
    └── wrapper        

```

### 注意点：

 - 过滤器 **@WebFilter** 失效：需要在启动类添加 **@ServletComponentScan** // 扫描过滤器组件

 - 过滤器 **@Order(value = 2)** 值越大优先执行过滤器

