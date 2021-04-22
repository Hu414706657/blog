---
title: RabbitMq
date: 2019-08-15 10:46:58
tags:
    - 消息队列
---

# 基于Docker安装RibbitMq

```text
version: '3.1'
services:
  rabbitmq:
    restart: always
    image: rabbitmq:management
    container_name: rabbitmq
    ports:
      - 5672:5672
      - 15672:15672
    environment:
      TZ: Asia/Shanghai
      RABBITMQ_DEFAULT_USER: rabbit
      RABBITMQ_DEFAULT_PASS: 123456
    volumes:
      - ./data:/var/lib/rabbitmq
```

# RabbitMQ WebUI

访问地址： http://ip:15672

# 最佳实践

## messageProvider

**POM**
```JAVA
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

**application.yml**
```text
spring:
  application:
    name: spring-boot-ribbitmq-provider
  rabbitmq:
    host: 192.168.2.129
    port: 5672
    username: rabbit
    password: 123456
```

**config**
创建一个名为`helloRabbit`的队列

```java
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Author: 胡文良
 * @Date: 2019/5/31 11:59
 */
@Configuration
public class RabbitMQConfiguration {
    @Bean
    public Queue queue() {
        return new Queue("helloRabbit");
    }
}
```

**消息推送**
```java
import org.springframework.amqp.core.AmqpTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.Date;

/**
 * @Author: 胡文良
 * @Date: 2019/5/31 12:01
 */

@Component
public class RabbitProvider {

    @Autowired
    private AmqpTemplate amqpTemplate;

    public void send() {
        String context = "hello" + new Date();
        System.out.println("Provider: " + context);
        amqpTemplate.convertAndSend("helloRabbit", context);
    }
}
```

## messageConsumer


**POM**
```JAVA
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

**application.yml**
```text
spring:
  application:
    name: spring-boot-ribbitmq-consumer
  rabbitmq:
    host: 192.168.2.129
    port: 5672
    username: rabbit
    password: 123456
```

**消息接收者**
```java
import org.springframework.amqp.rabbit.annotation.RabbitHandler;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @Author: 胡文良
 * @Date: 2019/5/31 13:49
 */

@Component
@RabbitListener(queues = "helloRabbit")
public class RabbitConsumer {
    @RabbitHandler
    public void process(String message) {
        System.out.println("Consumer: " + message);
    }
}
```
`@RabbitListener(queues = "helloRabbit")`:监听的队列