---
title: Hibernate
date: 2019-07-19 16:02:31
tags:
    - 数据库
---

# 概述
Hibernate是一个开放源代码的对象关系映射框架，它对JDBC进行了非常轻量级的对象封装，它将POJO与数据库表建立映射关系，是一个全自动的orm框架，hibernate可以自动生成SQL语句，自动执行，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。Hibernate可以应用在任何使用JDBC的场合，既可以在Java的客户端程序使用，也可以在Servlet/JSP的Web应用中使用。
Hibernate 是传统 Java 对象和数据库服务器之间的桥梁，用来处理基于 O/R 映射机制和模式的那些对象。

![](https://atts.w3cschool.cn/attachments/image/wk/hibernate/hibernate_position.jpg)

# Hibernate 优势

- Hibernate 使用 XML 文件来处理映射 Java 类别到数据库表格中，并且不用编写任何代码。
- 为在数据库中直接储存和检索 Java 对象提供简单的 APIs。
- 如果在数据库中或任何其它表格中出现变化，那么仅需要改变 XML 文件属性。
- 抽象不熟悉的 SQL 类型，并为我们提供工作中所熟悉的 Java 对象。
- Hibernate 不需要应用程序服务器来操作。
- 操控你数据库中对象复杂的关联。
- 最小化与访问数据库的智能提取策略。
- 提供简单的数据询问。

# Hibernate 架构
Hibernate 架构是分层的，作为数据访问层，你不必知道底层 API 。Hibernate 利用数据库以及配置数据来为应用程序提供持续性服务（以及持续性对象）。

![](https://atts.w3cschool.cn/attachments/image/wk/hibernate/hibernate_architecture.jpg)

## 配置对象

配置对象是你在任何 Hibernate 应用程序中创造的第一个 Hibernate 对象，并且经常只在应用程序初始化期间创造。它代表了 Hibernate 所需一个配置或属性文件。配置对象提供了两种基础组件。

- **数据库连接**：由 Hibernate 支持的一个或多个配置文件处理。这些文件是 **hibernate.properties** 和 **hibernate.cfg.xml**。
- **类映射设置**：这个组件创造了 Java 类和数据库表格之间的联系。

## SessionFactory 对象

配置对象被用于创造一个 SessionFactory 对象，使用提供的配置文件为应用程序依次配置 Hibernate，并允许实例化一个会话对象。SessionFactory 是一个线程安全对象并由应用程序所有的线程所使用。

SessionFactory 是一个重量级对象所以通常它都是在应用程序启动时创造然后留存为以后使用。每个数据库需要一个 SessionFactory 对象使用一个单独的配置文件。所以如果你使用多种数据库那么你要创造多种 SessionFactory 对象。

## Session 对象

一个会话被用于与数据库的物理连接。Session 对象是轻量级的，并被设计为每次实例化都需要与数据库的交互。持久对象通过 Session 对象保存和检索。

Session 对象不应该长时间保持开启状态因为它们通常情况下并非线程安全，并且它们应该按照所需创造和销毁。

## Transaction 对象

一个事务代表了与数据库工作的一个单元并且大部分 RDBMS 支持事务功能。在 Hibernate 中事务由底层事务管理器和事务（来自 JDBC 或者 JTA）处理。

这是一个选择性对象，Hibernate 应用程序可能不选择使用这个接口，而是在自己应用程序代码中管理事务。

## Query 对象

Query 对象使用 SQL 或者 Hibernate 查询语言（HQL）字符串在数据库中来检索数据并创造对象。一个查询的实例被用于连结查询参数，限制由查询返回的结果数量，并最终执行查询。

## Criteria 对象

Criteria 对象被用于创造和执行面向规则查询的对象来检索对象。

#### @Modifying注解
```text
这个注解是通知jpa，这是一个update或者delete操作，在更新或者删除操作时，此注解必须加，否则会抛出下面异常

org.springframework.dao.InvalidDataAccessApiUsageException: org.hibernate.hql.internal.QueryExecutionRequestException: Not supported for DML operations [update com.xhx.springboot.entity.Account set name=:name, money=:money,version=:version+1 where id=:id and version=:version]; nested exception is java.lang.IllegalStateException: org.hibernate.hql.internal.QueryExecutionRequestException: Not supported for DML operations [update com.xhx.springboot.entity.Account set name=:name, money=:money,version=:version+1 where id=:id and version=:version]

必须加事务，不加事务，会报下面错

org.springframework.dao.InvalidDataAccessApiUsageException: Executing an update/delete query; nested exception is javax.persistence.TransactionRequiredException: Executing an update/delete query

```

```sql
    @Query("SELECT ior from ComparisonShareUrl ior where ior.id = :uuid ")
    ComparisonShareUrl findByUuid(@Param("uuid") Integer uuid);

    @Query(nativeQuery = true, value = "SELECT * from comparison_share_url ior where ior.created_at > ?1 ORDER BY ior.created_at  LIMIT 1")
    ComparisonShareUrl findByTime(@Param("date") Date date);

    nativeQuery = true时，是可以执行原生sql语句，所谓原生sql，也就是说这段sql拷贝到数据库中，然后把参数值给一下就能运行
```