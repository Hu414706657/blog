---
title: SSH免密登陆
date: 2021-05-12 15:30:33
tags:
---

# SSH 三步解决免密登录

* 客户端生成密钥
* 将公钥生成至服务器
* 测试登陆

## 1. 客户端生成公私钥

* 命令：(一路回车默认即可)

``` text
ssh-keygen -t rsa -C "example@test.com"
```

公钥位置：

* mac：～/.ssh/id_rsa.pub
* window：C:\Users\Administrator\.ssh\id_rsa.pub

私钥位置：

* mac:～/.ssh/id_rsa
* window：C:\Users\Administrator\.ssh\id_rsa

## 2. 将公钥生成至服务器

将id_rsa.pub文件内容拷贝至服务器 .ssh/authorized_keys 文件中

## 登陆服务器验证

``` 
ssh root@ip
```

## GitHub免密克隆代码

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/SSH%E5%85%8D%E5%AF%86%E7%99%BB%E9%99%86.png)