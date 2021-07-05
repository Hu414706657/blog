---
title: FormData实现文件异步上传
date: 2021-07-05 16:09:55
tags:
---

## 概述
**`FormData`** 接口提供了一种表示表单数据的键值对 `key/value` 的构造方式，并且可以轻松的将数据通过[`XMLHttpRequest.send()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/send) 方法发送出去，本接口和此方法都相当简单直接。如果送出时的编码类型被设为 `"multipart/form-data"`，它会使用和表单一样的格式。


[文档说明](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData)

## 构造函数
**`FormData()`** 构造函数用于创建一个新的[`FormData`](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData)对象。

```javascript
var formData = new FormData(form)
//HTML上的form表单元素——当指定了，这种方式创建的FormData对象会自动将form中的表单值也包含进去，包括文件内容也会被编码之后包含进去。

var formData = new FormData(); // 当前为空
```

## 方法

- FormData.append()
FormData 接口的 **`append()`** 方法 会添加一个新值到 FormData 对象内的一个已存在的键中，如果键不存在则会添加该键。
FormData.set 和 **`append()`** 的区别在于，如果指定的键已经存在， FormData.set 会使用新值覆盖已有的值，而 append() 会把新值添加到已有值集合的后面。
    ```javascript
    formData.append(name, value);
    formData.append(name, value, filename);//filename文件上传定义文件名
    ```
- FormData.delete()
FormData 接口的 **`delete()`** 方法会从 FormData 对象中删除指定键，即 key，和它对应的值，即 value。
    ```javascript
    formData.delete(name);
    ```
- FormData.get()
FormData的 **`get()`** 方法用于返回FormData对象中和指定的键关联的第一个值，如果你想要返回和指定键关联的全部值，那么可以使用 **`getAll()`** 方法。
    ```javascript
    formData.get(name); 
    ```
- FormData.getAll()
**`getAll()`** 方法会返回该 FormData 对象指定 key 的所有值。
    ```javascript
    formData.append('username', 'Chris');
    formData.append('username', 'Bob');
    formData.getAll('username'); // Returns ["Chris", "Bob"]
    ```

## FormData异步文件上传
```javascript
//id:上传文件input框的id
function uploadFile(id) {
        var formData = new FormData();
        formData.append("image", $(id)[0].files[0]);
        $.ajax({
            url: '/product_admin/insurances/upload_image',
            type: 'POST',
            //将formData对象作为参数进行上传
            data: formData,
            contentType: false,
            processData: false,
            success: function (data) {
                //文件上传成功后的处理
                alert("上传成功")
            }
        })
    }

```