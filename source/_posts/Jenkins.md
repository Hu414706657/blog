---
title: Jenkins
date: 2021-04-30 14:05:10
tags:
---

# 基于Docker-composea安装Jenkins

```test
version: '3.1'
services:
  jenkins:
    restart: always
    image: jenkinsci/jenkins
    container_name: jenkins
    ports:
      # 发布端口:宿主机ip:容器ip
      - 5000:8080
      # 基于 JNLP 的 Jenkins 代理通过 TCP 端口 50000 与 Jenkins master 进行通信
      - 50000:50000
    environment:
      TZ: Asia/Shanghai
    volumes:
      #数据卷位置，在当前文件加简历data文件
      - ./data:/var/jenkins_home
```

- 启动命令：docker-compose up -d
- 
