---
title: yapi部署
date: 2020-01-14 22:40:36
tags:
---

# 基于Docker-compose部署Yapi

```
version: '2.1'
services:
  yapi:
    image: mrjin/yapi:latest
#    build: ./
    container_name: yapi
    environment:
      - VERSION=1.5.6
      - LOG_PATH=/tmp/yapi.log
      - HOME=/home
      - PORT=3000
      - ADMIN_EMAIL=414706657@qq.com
      - DB_SERVER=mongo
      - DB_NAME=yapi
      - DB_PORT=27017
    # restart: always
    ports:
      - 3000:3000
    volumes:
      - ~/data/yapi/log/yapi.log:/home/vendors/log # log dir
    depends_on:
      - mongo
    entrypoint: "bash /wait-for-it.sh mongo:27017 -- entrypoint.sh"
    networks:
      - back-net
  mongo:
    image: mongo
    container_name: mongo
    # restart: always
    ports:
      - 127.0.0.1:27017:27017
    volumes:
      - ~/data/yapi/mongodb:/data/db #db dir
    networks:
      - back-net
networks:
  back-net:
    external: true
                     
```

请求地址：ip:3000
默认密码：ymfe.org

