---
title: Spring
date: 2019-07-23 10:11:57
tags:
    - spring
---

# 概述

Spring 是一个轻量级的 Java 开发框架，主要作用就是为代码“解耦”，降低代码间的耦合度, 解决企业级开发的复杂性问题。核心是控制反转（IoC）和面向切面编程（AOP）。`IoC`:IoC 使得主业务在相互调用过程中，不用再自己维护关系，即不用再自己创建要使用的对象了。而是由 Spring容器统一管理，自动“注入”。`AOP`:Spring根据代码特点允许通过分离应用的业务逻辑与系统级业务逻辑（例如日志和事务管理）进行开发,AOP 使得系统级服务得到了最大复用，程序员不用手工将系统级服务“混杂”到主业务逻辑中了，而是由 Spring 容器统一完成“织入”。

# Spring 的特点
**非侵入式**
所谓非侵入式是指，Spring 框架的 API 不会在业务逻辑上出现，即业务逻辑是 POJO。由于业务逻辑中没有 Spring 的 API，所以业务逻辑可以从 Spring 框架快速的移植到其他框架， 即与环境无关。

(POJO:的内在含义是指那些没有从任何类继承、也没有实现任何接口，更没有被其它框架侵入的java对象。)
**容器**
 Spring 作为一个容器，可以管理对象的生命周期、对象与对象之间的依赖关系。可以通过配置文件，来定义对象，以及设置与其他对象的依赖关系。

 **IoC**
 控制反转（Inversion of Control），即创建被调用者的实例不是由调用者完成，而是由 Spring 容器完成，并注入调用者。

当应用了 IoC，一个对象依赖的其它对象会通过被动的方式传递进来，而不是这个对象自己创建或者查找依赖对象。即，不是对象从容器中查找依赖，而是容器在对象初始化时不等对象请求就主动将依赖传递给它。

IoC 是一个概念，是一种思想，其实现方式多种多样。当前比较流行的实现方式有两种： `依赖注入`和`依赖查找`。依赖注入方式应用更为广泛。

 - 依赖查找：Dependency Lookup，DL，容器提供回调接口和上下文环境给组件，程序代码则需要提供具体的查找方式。比较典型的是依赖于 JNDI 系统的查找。
 - 依赖注入：Dependency Injection，DI，程序代码不做定位查询，这些工作由容器自行完成。 依赖注入 DI 是指程序运行过程中，若需要调用另一个对象协助时，无须在代码中创建被调用者，而是依赖于外部容器，由外部容器创建后传递给程序。 Spring 的依赖注入对调用者与被调用者几乎没有任何要求，完全支持 POJO 之间依赖关系的管理。

**AOP**
面向切面编程（AOP，Aspect Orient Programming），是一种编程思想，是面向对象编程 OOP 的补充。很多框架都实现了对 AOP 编程思想的实现。Spring 也提供了面向切面编程的丰富支持，允许通过分离应用的业务逻辑与系统级服务（例如日志和事务管理）进行开发。应用对象只实现它们应该做的——完成业务逻辑——仅此而已。它们并不负责其它的系统级关注点，例如日志或事务支持。

我们可以把日志、安全、事务管理等服务理解成一个“切面”，那么以前这些服务一直是直接写在业务逻辑的代码当中的，这有两点不好：首先业务逻辑不纯净；其次这些服务被很多业务逻辑反复使用，完全可以剥离出来做到复用。那么 AOP 就是这些问题的解决方案， 可以把这些服务剥离出来形成一个“切面”，以期复用，然后将“切面”动态的“织入”到业务逻辑中，让业务逻辑能够享受到此“切面”的服务。