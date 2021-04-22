---
title: SpringBoot集成Mybatis
date: 2019-08-29 09:46:45
tags:
---

# SpringBoot集成Mybatis

**添加依赖**

```java
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>

<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.0</version>
</dependency>
```

**application.properties配置**

```text
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/blog?useUnicode=true&characterEncoding=utf-8&useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=123456

mybatis.type-aliases-package=com.example.demo.entity
mybatis.mapperLocations=classpath:mapper/*.xml
```

*实体类*

```java
import lombok.Data;

/**
 * @Author: 胡文良
 * @Date: 2019/8/28 20:58
 */

@Data
public class User {
    private String id;
    private String userName;
    private String password;
    private int age;
}
```

**mapper接口**

```java 
/**
 * @Author: 胡文良
 * @Date: 2019/8/28 20:57
 */
import com.example.demo.entity.User;
import org.apache.ibatis.annotations.Mapper;


@Mapper
public interface UserMapper {
    User select(int id);
}
```

**xml文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.demo.service.UserMapper">

    <select id="select" parameterType="INTEGER" resultType="com.example.demo.entity.User">
        select
        b.id as id,
        b.user_name as userName,
        b.password as password,
        b.age as age
        from tb_user b
        where id=1
    </select>
    
</mapper>
```

**启动类**

```java
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("com.example.demo.service")
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

# SpringBoot集成Druid

**添加POM依赖**

```java
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.10</version>
</dependency>
```

**application.properties文件配置**

```text
#druid配置详情信息
# 初始化大小，最小，最大
spring.datasource.initialSize=5
spring.datasource.minIdle=1
spring.datasource.maxActive=50
# 配置获取连接等待超时的时间
spring.datasource.maxWait=60000
# 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
spring.datasource.timeBetweenEvictionRunsMillis=60000
# 配置一个连接在池中最小生存的时间，单位是毫秒
spring.datasource.minEvictableIdleTimeMillis=300000
spring.datasource.validationQuery=SELECT 1 FROM DUAL
spring.datasource.testWhileIdle=true
spring.datasource.testOnBorrow=false
spring.datasource.testOnReturn=false
# 打开PSCache，并且指定每个连接上PSCache的大小
spring.datasource.poolPreparedStatements=false
#spring.datasource.maxPoolPreparedStatementPerConnectionSize=20
# 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
spring.datasource.filters=stat,wall,log4j
# 通过connectProperties属性来打开mergeSql功能；慢SQL记录
spring.datasource.connectionProperties=druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000
# 合并多个DruidDataSource的监控数据
#spring.datasource.useGlobalDataSourceStat=true
```
打开浏览器，输入：http://localhost:8080/druid/index.html 浏览器显示效果


# springboot2.0集成hikaricp

**概述**

`HikariCP`是数据库连接池,而且是号称史上最快的,而且目前来看确实是这样的,SpringBoot2.0也已经采用HikariCP作为默认连接池配置.

**HikariCP配置**

```text
# Hikari will use the above plus the following to setup connection pooling
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.maximum-pool-size=15
spring.datasource.hikari.auto-commit=true
spring.datasource.hikari.idle-timeout=30000
spring.datasource.hikari.pool-name=HikariCP
spring.datasource.hikari.max-lifetime=1800000
spring.datasource.hikari.connection-timeout=30000
```

如图展示配置成功

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/Hikaricp.png)