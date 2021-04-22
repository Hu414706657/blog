---
title: Mybatis
date: 2019-07-17 15:39:57
tags:
    数据库
---

# Mybatis

## jdbc 问题总结如下： 
- 数据库连接创建、释放频繁造成系统资源浪费,从而影响系统性能。如果使用数据库连接池可解决此问题。 
- sql语句在代码中硬编码,造成代码不易维护,实际应用中sql变化的可能较大,sql变动需要改变java代码。 
- 使用 preparedStatement向占有位符号传参数存在硬编码,因为sql语句的where条件不一定,可能多也可能少,修改sql还要修改代码,系统不易维护。
- 对结果集解析存在硬编码(查询列名),sql变化导致解析代码变化,系统不易维护,如果能将数据库记录封装成pojo对象解析比较方便。

## Mybatis解决 jdbc编程的间题
- 数据库连接创建、释放频繁造成系统资源浪费从而影响系统性能,如果使用数据库连接池可解决此问题
解决:在 sqllMapConfig.xm中配置数据连接池,使用连接池管理数据库链接。
- sqll语句写在代码中造成代码不易维护,实际应用sql变化的可能较大,sql变动需要改变java代码。
解决:将sqll语句配置在 XXXXmapperxm文件中与java代码分离。
- 向sql语句传参数麻烦,因为sql语句的 where条件不一定,可能多也可能少,占位符需要和参数一一对应
解决: Mybatis自动将java对象映射至sql语句,通过 statement中的parameterType定义输入参数的类型。
- sql变化导致解析代码变化,且解析前需要遍历,如果能将数据库记录封装成pojo对象解析比较方便。
解决: Mybatis自动将sqll执行结果映射至java对象,通过 statement中的resultType定义输出结果的类型。



## Mybatis 架构　　　　

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/%E6%9E%B6%E6%9E%84%E5%9B%BE-1.png)

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/%E6%9E%B6%E6%9E%84%E5%9B%BE.png) 



## Mybatis和Hibernate本质区别和应用场景
- Mybatis和 hibernate不同,它不完全是一个ORM框架,因为 MyBatis需要程序员自己编写Sq语句。 mybatis可以通过XML或注解方式灵活配置要运行的sq语句,并将java对象和sql语句映射生成最终执行的sq,最后将sq执行的结果再映射生成java对象。
- Mybatis学习门槛低,简单易学,程序员直接编写原生态sql,可严格控制sql执行性能,灵活度高,非常适合对关系数据模型要求不高的软件开发例如：互联网软件、企业运营类软件等,因为这类软件需求变化频繁,一但需求变化要求成果输出迅速。但是灵活的前提是 mybatis无法做到数据库无关性,如果需要实现支持多种数据库的软件则需要自定义多套sq映射文件,工作量大。


- Hibernate对象/关系映射能力强,数据库无关性好,对于关系模型要求高的软件(例如需求固定的定制化软件)如果用 hibernate开发可以节省很多代码,提高效率。但是 Hibernate的学习门槛高,要精通门槛更高,而且怎么设计O/R映射,在性能和对象模型之间如何权衡,以及怎样用好 Hibernate需要具有很强的经验和能力才行

总之,按照用户的需求在有限的资源环境下只要能做出维护性、扩展性良好
的软件架构都是好架构,所以框架只有适合才是最好。

# mybatis中主要类和接口
- Configuration：将mybatis配置文件中的信息保存到该类中
- SqlSessionFactory：解析Configuration类中的配置信息，获取SqlSession
- SqlSession：负责和数据库交互，完成增删改查
- Executor：mybatis的调度核心，负责SQL的生成
- StatementHandler：封装了JDBC的statement操作
- ParameterHandler：负责完成JavaType到jdbcType的转换
- ResultSetHandler：负责完成结果集到Java Bean的转换
- MappedStatement：代表一个select|update|insert|delete元素
- SqlSource：根据传入的ParamterObject生成SQL
- BoundSql：包含SQL和参数信息


# Spring 整合 MyBatis
**POM**
```java
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.2.8</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.1</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>4.3.17.RELEASE</version>
</dependency>
```
主要增加了 3 个依赖
- org.mybatis:mybatis
- org.mybatis:mybatis-spring
- org.springframework:spring-jdbc

**创建 MyBatis 配置文件**
创建一个名为 `mybatis-config.xml` 的配置文件，内容如下：
```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 全局参数 -->
    <settings>
        <!-- 打印 SQL 语句 -->
        <setting name="logImpl" value="STDOUT_LOGGING" />

        <!-- 使全局的映射器启用或禁用缓存。 -->
        <setting name="cacheEnabled" value="false"/>

        <!-- 全局启用或禁用延迟加载。当禁用时，所有关联对象都会即时加载。 -->
        <setting name="lazyLoadingEnabled" value="true"/>

        <!-- 当启用时，有延迟加载属性的对象在被调用时将会完全加载任意属性。否则，每种属性将会按需要加载。 -->
        <setting name="aggressiveLazyLoading" value="true"/>

        <!-- 是否允许单条 SQL 返回多个数据集 (取决于驱动的兼容性) default:true -->
        <setting name="multipleResultSetsEnabled" value="true"/>

        <!-- 是否可以使用列的别名 (取决于驱动的兼容性) default:true -->
        <setting name="useColumnLabel" value="true"/>

        <!-- 允许 JDBC 生成主键。需要驱动器支持。如果设为了 true，这个设置将强制使用被生成的主键，有一些驱动器不兼容不过仍然可以执行。 default:false  -->
        <setting name="useGeneratedKeys" value="false"/>

        <!-- 指定 MyBatis 如何自动映射 数据基表的列 NONE：不映射 PARTIAL：部分 FULL:全部  -->
        <setting name="autoMappingBehavior" value="PARTIAL"/>

        <!-- 这是默认的执行类型 （SIMPLE: 简单； REUSE: 执行器可能重复使用prepared statements语句；BATCH: 执行器可以重复执行语句和批量更新） -->
        <setting name="defaultExecutorType" value="SIMPLE"/>

        <!-- 使用驼峰命名法转换字段。 -->
        <setting name="mapUnderscoreToCamelCase" value="true"/>

        <!-- 设置本地缓存范围 session:就会有数据的共享 statement:语句范围 (这样就不会有数据的共享 ) defalut:session -->
        <setting name="localCacheScope" value="SESSION"/>

        <!-- 设置 JDBC 类型为空时,某些驱动程序 要指定值, default:OTHER，插入空值时不需要指定类型 -->
        <setting name="jdbcTypeForNull" value="NULL"/>
    </settings>
</configuration>
```

**Spring 集成 MyBatis**
创建一个名为 `spring-context-mybatis.xml` 的 Spring 配置文件，内容如下：
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!-- 配置 SqlSession -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!-- 用于配置对应实体类所在的包，多个 package 之间可以用 ',' 号分割 -->
        <property name="typeAliasesPackage" value="com.funtl.my.shop.domain"/>
        <!-- 用于配置对象关系映射配置文件所在目录 -->
        <property name="mapperLocations" value="classpath:/mapper/**/*.xml"/>
        <property name="configLocation" value="classpath:/mybatis-config.xml"></property>
    </bean>

    <!-- 扫描 Mapper -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.funtl.my.shop.web.admin.dao" />
    </bean>
</beans>
```


