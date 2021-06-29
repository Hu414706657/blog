---
title: informix数据库简单实践
date: 2021-06-29 11:46:07
tags:
---

## 介绍：
Informix是IBM公司出品的关系数据库管理系统（RDBMS）家族。作为一个集成解决方案，它被定位为作为IBM在线事务处理（OLTP）旗舰级数据服务系统。

## 基于docker部署Informix
```text
镜像拉取
docker pull ibmcom/informix-developer-database

首次启动 Informix Docker 容器
docker run -it --name ifx -h ifx --privileged -e LICENSE=accept -p 9088:9088 -p 9089:9089 -p 27017:27017 -p 27018:27018 -p 27883:27883  ibmcom/informix-developer-database:latest

启动 Informix Docker 容器
docker start ifx

```
[更多说明](https://github.com/informix/informix-dockerhub-readme/blob/master/14.10.FC5/informix-developer-database.md)


## 基于dbacces创建数据库
- 进入容器
```text
docker exec -it 容器ID /bin/bash
```

- 输入<code>dbaccess</code>进入选择窗口
- Database -> Create -> 输入数据库名
ps:回车后选择Log选择Log文件(数据库默认选择None，会导致创建失败)，exit->Create-new-database

