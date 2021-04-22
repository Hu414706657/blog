---
title: Swagger2
date: 2019-08-28 10:08:29
tags:
---

Swagger2 是一个规范和完整的框架，用于生成、描述、调用和可视化Restful风格的web服务

# SpringBoot集成Swagger2

**添加依赖**
```java
    <!-- Swagger2 Begin -->
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger2</artifactId>
        <version>2.9.2</version>
    </dependency>
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger-ui</artifactId>
        <version>2.9.2</version>
    </dependency>
    <!-- Swagger2 End -->

```

**创建一个Java 配置类**

```java

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

/**
 * @Author: 胡文良
 * @Date: 2019/8/28 10:50
 */
@Configuration
@EnableSwagger2
public class Swagger2Configuration {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.demo.swagger2.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("标题：某公司_用户信息管理系统_接口文档")
                .description("描述：用于管理集团旗下公司的人员信息,具体包括XXX,XXX模块...")
                .termsOfServiceUrl("http://www.baidu.com")
                .version("1.0.0")
                .build();
    }
}
```

`RequestHandlerSelectors.basePackage("com.example.demo.swagger2.controller")`为 Controller 包路径，不然生成的文档扫描不到接口

`@EnableSwagger2`表示开启 Swagger

**响应数据**
```java

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

/**
 * @Author: 胡文良
 * @Date: 2019/8/28 11:34
 */

@Data
@ApiModel(value="用户信息", description="用户描述")
public class User {

    @ApiModelProperty(value="用户ID")
    private String userId;

    @ApiModelProperty(value="用户名")
    private String username;

    @ApiModelProperty(value="密码")
    private String password;

    public User() {
    }

    public User(String userId, String username, String password) {
        this.userId = userId;
        this.username = username;
        this.password = password;
    }
}

```


**使用 Swagger2 在 Controller 中增加 Swagger2 相关注解，代码如下：**

```java

import com.example.demo.swagger2.entity.User;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiImplicitParam;
import io.swagger.annotations.ApiOperation;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;

/**
 * @Author: 胡文良
 * @Date: 2019/8/28 11:35
 */

@Api(tags = "用户信息管理")
@RestController
@RequestMapping("/user/")
public class UserController {

    private final static List<User> userList = new ArrayList<>();

    {
        userList.add(new User("1", "admin", "123456"));
        userList.add(new User("2", "jacks", "111111"));
    }

    @ApiOperation("获取列表")
    @GetMapping("list")
    public List userList() {
        return userList;
    }

    @ApiOperation("新增用户")
    @PostMapping("save")
    public boolean save(User user) {
        return userList.add(user);
    }

    @ApiOperation("更新用户")
    @ApiImplicitParam(name = "user", value = "单个用户信息", dataType = "User")
    @PutMapping("update")
    public boolean update(User user) {
        return userList.remove(user) && userList.add(user);
    }

    @ApiOperation("批量删除")
    @ApiImplicitParam(name = "users", value = "N个用户信息", dataType = "List<User>")
    @DeleteMapping("delete")
    public boolean delete(@RequestBody List<User> users) {
        return userList.removeAll(users);
    }
}

```

**访问 Swagger2,访问地址：http://ip:port/swagger-ui.html**
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/swagger2.png)

# Swagger 常用注解说明

- `@Api`：修饰整个类，描述 Controller 的作用
- `@ApiOperation`：描述一个类的一个方法，或者说一个接口
- `@ApiParam`：单个参数描述
- `@ApiModel`：用对象来接收参数
- `@ApiProperty`：用对象接收参数时，描述对象的一个字段
- `@ApiResponse`：HTTP 响应其中 1 个描述
- `@ApiResponses`：HTTP 响应整体描述
- `@ApiIgnore`：使用该注解忽略这个API
- `@ApiError`：发生错误返回的信息
- `@ApiImplicitParam`：一个请求参数
- `@ApiImplicitParams`：多个请求参数

### `@ApiImplicitParams`

说明：用在请求的方法上，表示一组参数说明；`@ApiImplicitParam`：用在 `@ApiImplicitParams` 注解中，指定一个请求参数的各个方面

常用参数：

- name：参数名，参数名称可以覆盖方法参数名称，路径参数必须与方法参数一致
- value：参数的汉字说明、解释
- required：参数是否必须传，默认为 false （路径参数必填）
- paramType：参数放在哪个地方
  - header 请求参数的获取：`@RequestHeader`
  - query 请求参数的获取：`@RequestParam`
  - path（用于 restful 接口）--> 请求参数的获取：`@PathVariable`
  - body（不常用）
  - form（不常用）
- dataType：参数类型，默认 String，其它值 dataType="Integer"
- defaultValue：参数的默认值

其他参数（`@ApiImplicitParam`）：

- allowableValues 限制参数的可接受值。1.以逗号分隔的列表 2.范围值 3.设置最小值/最大值
- access 允许从API文档中过滤参数。
- allowMultiple 指定参数是否可以通过具有多个事件接受多个值，默认为 false
- example 单个示例
- examples 参数示例。仅适用于 BodyParameters

```java
@ResponseBody
@PostMapping(value="/login")
@ApiOperation(value = "登录检测", notes="根据用户名、密码判断该用户是否存在")
@ApiImplicitParams({
    @ApiImplicitParam(name = "name", value = "用户名", required = false, paramType = "query", dataType = "String"),
    @ApiImplicitParam(name = "pass", value = "密码", required = false, paramType = "query", dataType = "String")
})
public UserModel login(@RequestParam(value = "name", required = false) String account,
@RequestParam(value = "pass", required = false) String password){}
```



### `@ApiParam`

说明：用在请求方法中，描述参数信息

常用参数：

- name：参数名称，参数名称可以覆盖方法参数名称，路径参数必须与方法参数一致
- value：参数的简要说明。
- defaultValue：参数默认值
- required：属性是否必填，默认为 false （路径参数必须填）

```java
@ResponseBody
@PostMapping(value="/login")
@ApiOperation(value = "登录检测", notes="根据用户名、密码判断该用户是否存在")
public UserModel login(@ApiParam(name = "model", value = "用户信息Model") UserModel model){}
```



### `@ApiModel`

说明：用于响应类上，表示一个返回响应数据的信息（这种一般用在 POST 创建的时候，使用 `@RequestBody` 这样的场景，请求参数无法使用 `@ApiImplicitParam` 注解进行描述的时候）；`@ApiModelProperty`：用在属性上，描述响应类的属性

其他参数(@ApiModelProperty)：

- value 此属性的简要说明。
- name 允许覆盖属性名称
- allowableValues 限制参数的可接受值。1.以逗号分隔的列表 2.范围值 3.设置最小值/最大值
- access 允许从 API 文档中过滤属性。
  - notes 目前尚未使用。
- dataType 参数的数据类型。可以是类名或者参数名，会覆盖类的属性名称。
- required 参数是否必传，默认为 false
- position 允许在类中对属性进行排序。默认为 0
- hidden 允许在 Swagger 模型定义中隐藏该属性。
- example 属性的示例。
- readOnly 将属性设定为只读。
- reference 指定对相应类型定义的引用，覆盖指定的任何参数值

```java
@ApiModel(value="用户登录信息", description="用于判断用户是否存在")
public class UserModel implements Serializable{

   private static final long serialVersionUID = 1L;

   /**
    * 用户名
    */
   @ApiModelProperty(value="用户名")
   private String account;

   /**
     * 密码
     */
    @ApiModelProperty(value="密码")
   private String password;
}
```



### `@ApiResponses`

说明：用在请求的方法上，表示一组响应；`@ApiResponse`：用在 `@ApiResponses` 中，一般用于表达一个错误的响应信息

常用参数：

- code：数字，例如 400
- message：信息，例如 "请求参数没填好"
- response：抛出异常的类

```java
@ResponseBody
@PostMapping(value="/update/{id}")
@ApiOperation(value = "修改用户信息",notes = "打开页面并修改指定用户信息")
@ApiResponses({
    @ApiResponse(code=400,message="请求参数没填好"),
    @ApiResponse(code=404,message="请求路径没有或页面跳转路径不对")
})
public JsonResult update(@PathVariable String id, UserModel model){}
```