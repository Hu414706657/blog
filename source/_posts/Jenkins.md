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
    image: jenkins/jenkins
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
- 安装过程中会出现 Docker 数据卷 权限问题，用以下命令解决：chown -R 1000 ./data

### 解锁 Jenkins
- Jenkins 第一次启动时需要输入一个初始密码用以解锁安装流程，使用 docker logs jenkins 即可方便的查看到初始密码

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/jenkins_1.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/jenkins_2.png)


### Jenkins 插件下载缓慢或失败解决办法
- Jenkins设置清华镜像加速:https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/jenkins_3.webp)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/jenkins_4.webp)

### Jenkins配置jdk和maven
- 将jdk和maven拷贝至数据卷data目录下
- 使用docker exec -it 容器id /bin/bansh 进入容器/var/jenkins_home目录下确认是否存在文件
- 将JAVA_HOME和MAVEN_HOME配置为容器地址
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/jenkins_5.png)