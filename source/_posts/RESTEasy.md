---
title: RESTEasy
date: 2019-08-27 10:00:50
tags:
---

# 概述

RESTEasy是JBoss的一个开源项目，提供各种框架帮助你构建RESTful Web Services和RESTful Java应用程序。它是JAX-RS规范的一个完整实现并通过JCP认证。作为一个JBOSS的项目，它当然能和JBOSS应用服务器很好地集成在一起。但是，它也能在任何运行JDK5或以上版本的Servlet容器中运行。RESTEasy还提供一个RESTEasy JAX-RS客户端调用框架。能够很方便与EJB、Seam、Guice、Spring和Spring MVC集成使用。支持在客户端与服务器端自动实现GZIP解压缩

# 规范说明

| 注解	| 描述 |
| ---- | ---- |
| @Path				| 类或者方法的相对地址，标注在类或者方法上                       |
| @GET				| HTTP GET请求用，用来获取资源                                   |
| @PUT				| HTTP PUT 请求，用来创建资源                                    |
| @POST				| HTTP POST 请求, 用来创建或者更新资源                           |
| @DELETE			| 	HTTP DELETE请求, 用来删除资源                                |
| @HEAD				| HTTP HEAD请求, 用来获取一个接口是否可用的状态                  |
| @Produces			| 返回数据的格式比如APPLICATION/XML, TEXT/HTML, APPLICATION/JSON |
| @Consumes			| 请求数据格式 , 如json                                          |
| @PathParam		| 	绑定url里面的参数                                            |
| @QueryParam		| 	绑定url后面的参数（？后面的）                                |
| @MatrixParam		| 绑定包含多个 property (属性)=value(值) 方法参数表达式          |
| @HeaderParam		| 给header里面传参                                               |
| @CookieParam		| 给cookie里面传参                                               |
| @FormParam		| 	给form传参                                                   |
| @DefaultValue		| 给参数一个默认值                                               |
| @Context			| 可以注入HttpRequest，HttpResponse用                            |


**博客推荐**
[Spring Boot 集成 resteasy篇 — jax-rs初步介绍和spring boot集成](https://blog.csdn.net/u011410529/article/details/77503918)
[Spring Boot 集成 resteasy篇 — jax-rs常用注解介绍](https://blog.csdn.net/u011410529/article/details/77505238)