---
title: MySql
date: 2019-08-23 16:49:34
tags:
   - 数据库
---

# 概述

MySQL是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，目前属于 Oracle 旗下产品。MySQL 是最流行的关系型数据库管理系统之一

# 基于Docker安装MySQL
```text
ersion: '3.1'
services:
  mysql:
    restart: always
    image: mysql:5.7.22
    container_name: mysql
    ports:
      - 3306:3306
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123456
    command:
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_names=1
      --max_allowed_packet=128M
      --sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO"
    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:

```

# 基本指令
连接到本机上的MySQ：
```text
mysql -uroot -p /*如果刚安装好MySQL，root是没有密码的*/
```
连接到远程主机上的MySQL：
```text
mysql> mysql -h192.168.206.100 -uroot -p12345678; /*u与root可以不加空格*/
```
退出MySQL命令： exit 或 quit（回车）：
```text
mysql> exit/quit;
```
指定要操作的数据库
```text
USE 数据库名；
```
查看数据表
```text
SHOW CREATE TABLE 数据表名；
```

