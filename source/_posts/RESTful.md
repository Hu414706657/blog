---
title: RESTful API
date: 2019-08-27 09:19:45
tags:
---

# URL 设计

- 动词 + 宾语

RESTful 的核心思想就是，客户端发出的数据操作指令都是"动词 + 宾语"的结构。比如，`GET /articles`这个命令，`GET`是动词，`/articles`是宾语

动词通常就是五种 HTTP 方法，对应 CRUD 操作。

    GET：读取（Read）
    POST：新建（Create）
    PUT：更新（Update）
    PATCH：更新（Update），通常是部分更新
    DELETE：删除（Delete）
根据 HTTP 规范，动词一律大写。

- 动词的覆盖
有些客户端只能使用`GET`和`POST`这两种方法。服务器必须接受`POST`模拟其他三个方法（`PUT`、`PATCH`、`DELETE`）。

这时，客户端发出的 HTTP 请求，要加上`X-HTTP-Method-Override`属性，告诉服务器应该使用哪一个动词，覆盖`POST`方法。

    POST /api/Person/4 HTTP/1.1  
    X-HTTP-Method-Override: PUT

上面代码中，`X-HTTP-Method-Override`指定本次请求的方法是PUT，而不是POST。

- 宾语必须是名词
宾语就是 API 的 URL，是 HTTP 动词作用的对象。它应该是名词，不能是动词。比如，`/articles`这个 URL 就是正确的，而下面的 URL 不是名词，所以都是错误的。


    /getAllCars
    /createNewCar
    /deleteAllRedCars

- 复数 URL

既然 URL 是名词，那么应该使用复数，还是单数？

这没有统一的规定，但是常见的操作是读取一个集合，比如`GET /articles`（读取所有文章），这里明显应该是复数。

为了统一起见，建议都使用复数 URL，比如`GET /articles/2`要好于`GET /article/2`。


- 避免多级 URL

常见的情况是，资源需要多级分类，因此很容易写出多级的 URL，比如获取某个作者的某一类文章。

    GET /authors/12/categories/2

这种 URL 不利于扩展，语义也不明确，往往要想一会，才能明白含义。

更好的做法是，除了第一级，其他级别都用查询字符串表达。

    GET /authors/12?categories=2

下面是另一个例子，查询已发布的文章。你可能会设计成下面的 URL。

    GET /articles?published=true

`POST`和`PUT`在创建资源的区别在于，所创建的资源的名称(URI)是否由客户端决定。 例如为我的博文增加一个java的分类，生成的路径就是分类名`/categories/java`，那么就可以采用PUT方法。不过很多人直接把POST、GET、PUT、DELETE直接对应上CRUD，例如在一个典型的rails实现的RESTful应用中就是这么做的。

# 状态码

- 状态码必须精确
客户端的每一次请求，服务器都必须给出回应。回应包括 HTTP 状态码和数据两部分。

HTTP 状态码就是一个三位数，分成五个类别。

    1xx：相关信息
    2xx：操作成功
    3xx：重定向
    4xx：客户端错误
    5xx：服务器错误

**GET**

    安全且幂等
    获取表示
    变更时获取表示（缓存）
    200（OK） - 表示已在响应中发出
    204（无内容） - 资源有空表示
    301（Moved Permanently） - 资源的URI已被更新
    303（See Other） - 其他（如，负载均衡）
    304（not modified）- 资源未更改（缓存）
    400 （bad request）- 指代坏请求（如，参数错误）
    404 （not found）- 资源不存在
    406 （not acceptable）- 服务端不支持所需表示
    500 （internal server error）- 通用错误响应
    503 （Service Unavailable）- 服务端当前无法处理请求

**POST**

    不安全且不幂等
    使用服务端管理的（自动产生）的实例号创建资源
    创建子资源
    部分更新资源
    如果没有被修改，则不过更新资源（乐观锁）
    200（OK）- 如果现有资源已被更改
    201（created）- 如果新资源被创建
    202（accepted）- 已接受处理请求但尚未完成（异步处理）
    301（Moved Permanently）- 资源的URI被更新
    303（See Other）- 其他（如，负载均衡）
    400（bad request）- 指代坏请求
    404 （not found）- 资源不存在
    406 （not acceptable）- 服务端不支持所需表示
    409 （conflict）- 通用冲突
    412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）
    415 （unsupported media type）- 接受到的表示不受支持
    500 （internal server error）- 通用错误响应
    503 （Service Unavailable）- 服务当前无法处理请求
**PUT**

    不安全但幂等
    用客户端管理的实例号创建一个资源
    通过替换的方式更新资源
    如果未被修改，则更新资源（乐观锁）
    200 （OK）- 如果已存在资源被更改
    201 （created）- 如果新资源被创建
    301（Moved Permanently）- 资源的URI已更改
    303 （See Other）- 其他（如，负载均衡）
    400 （bad request）- 指代坏请求
    404 （not found）- 资源不存在
    406 （not acceptable）- 服务端不支持所需表示
    409 （conflict）- 通用冲突
    412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）
    415 （unsupported media type）- 接受到的表示不受支持
    500 （internal server error）- 通用错误响应
    503 （Service Unavailable）- 服务当前无法处理请求
**DELETE**

    不安全但幂等
    删除资源
    200 （OK）- 资源已被删除
    301 （Moved Permanently）- 资源的URI已更改
    303 （See Other）- 其他，如负载均衡
    400 （bad request）- 指代坏请求
    404 （not found）- 资源不存在
    409 （conflict）- 通用冲突
    500 （internal server error）- 通用错误响应
    503 （Service Unavailable）- 服务端当前无法处理请求

# 服务器回应

- 不要返回纯本文

API 返回的数据格式，不应该是纯文本，而应该是一个 JSON 对象，因为这样才能返回标准的结构化数据。所以，服务器回应的 HTTP 头的`Content-Type`属性要设为`application/json`。

客户端请求时，也要明确告诉服务器，可以接受 JSON 格式，即请求的 HTTP 头的`ACCEPT`属性也要设成`application/json`。下面是一个例子

    GET /orders/2 HTTP/1.1 
    Accept: application/json

- 发生错误时，不要返回 200 状态码
有一种不恰当的做法是，即使发生错误，也返回200状态码，把错误信息放在数据体里面，就像下面这样。

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
    "status": "failure",
    "data": {
        "error": "Expected at least two items in list."
    }
    }

上面代码中，解析数据体以后，才能得知操作失败。

这张做法实际上取消了状态码，这是完全不可取的。正确的做法是，状态码反映发生的错误，具体的错误信息放在数据体里面返回。下面是一个例子。

    HTTP/1.1 400 Bad Request
    Content-Type: application/json

    {
    "error": "Invalid payoad.",
    "detail": {
        "surname": "This field is required."
        }
    }

