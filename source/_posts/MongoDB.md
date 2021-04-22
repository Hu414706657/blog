---
title: MongoDB
date: 2019-10-16 10:21:31
tags:
---

## 基于docker安装MongoDB

```text
version: "3"
services:
 
  mongodb:
    image: mongo:4.1
    environment:
        MONGO_INITDB_ROOT_USERNAME: "root"
        MONGO_INITDB_ROOT_PASSWORD: "abc123"
        TZ: "Asia/Shanghai"
    volumes:
      - ./mongo:/data/db:rw
      - ./mongo/mongo_key:/mongo:rw
    ports:
       - "27017:27017"
    networks:
       - default
    command:
       --auth

      
networks:
  default:


```

**注意**：新建数据库需要创建用户才能指定数据库连接

```test
db.createUser(
  {
    user: "name",
    pwd: "password",
    roles: [ { role: "readWrite", db: "db_name" } ]
  }
)
```

**Built-In Roles（内置角色）：**

```text
1. 数据库用户角色：read、readWrite;
2. 数据库管理角色：dbAdmin、dbOwner、userAdmin；
3. 集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；
4. 备份恢复角色：backup、restore；
5. 所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
6. 超级用户角色：root 
// 这里还有几个角色间接或直接提供了系统超级用户的访问（dbOwner 、userAdmin、userAdminAnyDatabase）
7. 内部角色：__system
```

**具体角色：**

```text
Read：允许用户读取指定数据库
readWrite：允许用户读写指定数据库
dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile
userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户
clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。
readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。
root：只在admin数据库中可用。超级账号，超级权限。
```

## Boot集成MongoDB

**POM**

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

**application.properties**

```text
spring.data.mongodb.host=120.78.152.130
spring.data.mongodb.port=27017
spring.data.mongodb.database=db_demo
spring.data.mongodb.username=root
spring.data.mongodb.password=123456
```

**创建实体类对应数据库表**

```java
import lombok.Data;
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.data.mongodb.core.mapping.Field;

/**
 * @Author: 胡文良
 * @Date: 2019/10/15 14:01
 */

@Data
@Document(collection = "db_user")
public class User {

    @Id
    private String id;

    @Field("name")
    private String name;

    @Field("age")
    private String age;

    @Field("sex")
    private String sex;
}
```

**DAO创建查询方法**

```java
import com.example.demo.mongodb.entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.mongodb.core.MongoTemplate;
import org.springframework.stereotype.Repository;

import java.util.List;

/**
 * @Author: 胡文良
 * @Date: 2019/10/15 12:16
 */
@Repository
public class DemoDao extends MongoDbDao<User>{

    @Autowired
    private MongoTemplate template;

    public List<User> findList(){

        return template.findAll(User.class);
    }

}
```

## 常用的数据库查询方法提炼

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.mongodb.core.MongoTemplate;
import org.springframework.data.mongodb.core.query.Criteria;
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.data.mongodb.core.query.Update;

import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.util.List;

/**
 * @Author: 胡文良
 * @Date: 2019/10/15 14:20
 */
public abstract class MongoDbDao<T> {

    protected Logger logger = LoggerFactory.getLogger(MongoDbDao.class);

    /**
     * 反射获取泛型类型
     *
     * @return
     */
    protected abstract Class<T> getEntityClass();

    @Autowired
    private MongoTemplate mongoTemplate;

    /**
     *
     * 无条件查询全部
     */
    public List<T> findAll(){
        return this.mongoTemplate.findAll(this.getEntityClass());
    }

    /***
     * 根据id从几何中查询对象
     * @param id
     * @return
     */
    public T queryById(Integer id) {
        Query query = new Query(Criteria.where("_id").is(id));
        logger.info("-------------->MongoDB find start");
        return this.mongoTemplate.findOne(query, this.getEntityClass());
    }

    /**
     * 根据条件查询集合
     *
     * @param object
     * @return
     */
    public List<T> queryList(T object) {
        Query query = getQueryByObject(object);
        logger.info("-------------->MongoDB find start");
        return mongoTemplate.find(query, this.getEntityClass());
    }

    /**
     * 根据条件查询只返回一个文档
     *
     * @param object
     * @return
     */
    public T queryOne(T object) {
        Query query = getQueryByObject(object);
        logger.info("-------------->MongoDB find start");
        return mongoTemplate.findOne(query, this.getEntityClass());
    }

    /***
     * 根据条件分页查询
     * @param object
     * @param start 查询起始值
     * @param size  查询大小
     * @return
     */
    public List<T> getPage(T object, int start, int size) {
        Query query = getQueryByObject(object);
        query.skip(start);
        query.limit(size);
        logger.info("-------------->MongoDB queryPage start");
        return this.mongoTemplate.find(query, this.getEntityClass());
    }

    /***
     * 根据条件查询库中符合条件的记录数量
     * @param object
     * @return
     */
    public Long getCount(T object) {
        Query query = getQueryByObject(object);
        logger.info("-------------->MongoDB Count start");
        return this.mongoTemplate.count(query, this.getEntityClass());
    }

    /***
     * 删除对象
     * @param t
     * @return
     */
    public int delete(T t) {
        logger.info("-------------->MongoDB delete start");
        return (int) this.mongoTemplate.remove(t).getDeletedCount();
    }

    /**
     * 根据id删除
     *
     * @param id
     */
    public void deleteById(Integer id) {
        Criteria criteria = Criteria.where("_id").is(id);
        if (null != criteria) {
            Query query = new Query(criteria);
            T obj = this.mongoTemplate.findOne(query, this.getEntityClass());
            logger.info("-------------->MongoDB deleteById start");
            if (obj != null) {
                this.delete(obj);
            }
        }
    }

    /*MongoDB中更新操作分为三种
     * 1：updateFirst     修改第一条
     * 2：updateMulti     修改所有匹配的记录
     * 3：upsert  修改时如果不存在则进行添加操作
     * */

    /**
     * 修改匹配到的第一条记录
     * @param srcObj
     * @param targetObj
     */
    public void updateFirst(T srcObj, T targetObj){
        Query query = getQueryByObject(srcObj);
        Update update = getUpdateByObject(targetObj);
        logger.info("-------------->MongoDB updateFirst start");
        this.mongoTemplate.updateFirst(query,update,this.getEntityClass());
    }

    /***
     * 修改匹配到的所有记录
     * @param srcObj
     * @param targetObj
     */
    public void updateMulti(T srcObj, T targetObj){
        Query query = getQueryByObject(srcObj);
        Update update = getUpdateByObject(targetObj);
        logger.info("-------------->MongoDB updateFirst start");
        this.mongoTemplate.updateMulti(query,update,this.getEntityClass());
    }

    /***
     * 修改匹配到的记录，若不存在该记录则进行添加
     * @param srcObj
     * @param targetObj
     */
    public void updateInsert(T srcObj, T targetObj){
        Query query = getQueryByObject(srcObj);
        Update update = getUpdateByObject(targetObj);
        logger.info("-------------->MongoDB updateInsert start");
        this.mongoTemplate.upsert(query,update,this.getEntityClass());
    }

    /**
     * 将查询条件对象转换为query
     *
     * @param object
     * @return
     * @author Jason
     */
    private Query getQueryByObject(T object) {
        Query query = new Query();
        String[] fileds = getFiledName(object);
        Criteria criteria = new Criteria();
        for (int i = 0; i < fileds.length; i++) {
            String filedName = (String) fileds[i];
            Object filedValue = getFieldValueByName(filedName, object);
            if (filedValue != null) {
                criteria.and(filedName).is(filedValue);
            }
        }
        query.addCriteria(criteria);
        return query;
    }

    /**
     * 将查询条件对象转换为update
     *
     * @param object
     * @return
     * @author Jason
     */
    private Update getUpdateByObject(T object) {
        Update update = new Update();
        String[] fileds = getFiledName(object);
        for (int i = 0; i < fileds.length; i++) {
            String filedName = (String) fileds[i];
            Object filedValue =getFieldValueByName(filedName, object);
            if (filedValue != null) {
                update.set(filedName, filedValue);
            }
        }
        return update;
    }

    /***
     * 获取对象属性返回字符串数组
     * @param o
     * @return
     */
    private static String[] getFiledName(Object o) {
        Field[] fields = o.getClass().getDeclaredFields();
        String[] fieldNames = new String[fields.length];

        for (int i = 0; i < fields.length; ++i) {
            fieldNames[i] = fields[i].getName();
        }

        return fieldNames;
    }

    /***
     * 根据属性获取对象属性值
     * @param fieldName
     * @param o
     * @return
     */
    private static Object getFieldValueByName(String fieldName, Object o) {
        try {
            String e = fieldName.substring(0, 1).toUpperCase();
            String getter = "get" + e + fieldName.substring(1);
            Method method = o.getClass().getMethod(getter, new Class[0]);
            return method.invoke(o, new Object[0]);
        } catch (Exception var6) {
            return null;
        }
    }
}
```
## MongoTemplate 应用

查询操作，Query类型为筛选条件，在查询修改和删除中通用，Criteria是拼接条件，where().is()，也可以是其他的lt、lte、gt、gte、ne等等。

排序或者限制输出行数这些与where无关的在Query里设置q.limit()、q.skip()、q.with(Sort.by(Sort.Order.desc(sname)));

```java
Query q=new Query( Criteria.where("_id").is(id));
List<User> list = mongoTemplate.find(q, User.class);
```



**关于 insert 和 save 函数的区别**

```text
插入重复数据
　　insert: 若新增数据的主键已经存在，则会抛 org.springframework.dao.DuplicateKeyException 异常			    提示主键重复，不保存当前数据。 
　　save: 若新增数据的主键已经存在，则会对当前已经存在的数据进行修改操作。

批操作
　　insert: 可以一次性插入一整个列表，而不用进行遍历操作，效率相对较高 
　　save: 需要遍历列表，进行一个个的插入
```