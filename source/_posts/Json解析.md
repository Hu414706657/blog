---
title: Json解析
date: 2021-05-28 11:19:12
tags:
---

### fastJson解析

依赖添加：
```java
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.60</version>
</dependency>
```

解析：
```java

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.alibaba.fastjson.serializer.SerializerFeature;
import com.example.spring.boot.demo.entity.Entity;
import com.example.spring.boot.demo.entity.User;

import java.util.ArrayList;
import java.util.List;


/**
 * @author: 胡文良
 * @ClassName JsonDemo.java
 * @description:
 * @create: 2021-06-01 14:49
 **/
public class JsonDemo {
    public static void main(String[] args) {
        Entity entity = new Entity("字段1","字段2");
        String str = JSONObject.toJSONString(entity);
        System.out.println(str);
        
        Entity entity1 = new Entity("lisi", "321321");
        List<Entity> users = new ArrayList<Entity>();
        users.add(entity1);
        users.add(entity);
        String str1 = JSONObject.toJSONString(users);
        System.out.println(str1);

        User user = new User("user1","user2");
        entity.setUser(user);
        String str2 = JSONObject.toJSONString(entity);
        System.out.println(str1);


        str = "{\"tes\":\"字段1\",\"tes1\":\"字段2\"}";
        str1 = "[{\"tes\":\"zhangsan\",\"tes1\":\"123123\"},{\"tes\":\"lisi\",\"tes1\":\"321321\"}]";
        str2 = "{\"tes\":\"字段1\",\"tes1\":\"字段2\",\"user\":{\"str1\":\"user1\",\"str2\":\"user2\"}}";
        entity1 = JSONObject.parseObject(str, Entity.class);
        List<Entity> list = JSON.parseArray(str1, Entity.class);
        Entity entity2 = JSONObject.parseObject(str2, Entity.class);

        JSONObject.toJSONString(entity, SerializerFeature.WriteMapNullValue);

    }
}

```

Fastjson的SerializerFeature序列化属性
```text
QuoteFieldNames———-输出key时是否使用双引号,默认为true 
WriteMapNullValue——–是否输出值为null的字段,默认为false 
WriteNullNumberAsZero—-数值字段如果为null,输出为0,而非null 
WriteNullListAsEmpty—–List字段如果为null,输出为[],而非null 
WriteNullStringAsEmpty—字符类型字段如果为null,输出为”“,而非null 
WriteNullBooleanAsFalse–Boolean字段如果为null,输出为false,而非null
```





- @JsonProperty和@JSONField的区别
!()[https://www.codetd.com/article/11887797]