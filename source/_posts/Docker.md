---
title: Docker
date: 2019-08-15 10:42:23
tags:
---

# Docker 引擎

**Docker引擎是一个客户服务器(c/s)应用程序，主要组件：**

- 一种服务器，它是一种称为守护进程并且长时间运行的程序。
- REST API用于指定程序可以用来与守护进程通信的接口，并指示它做什么。
- 一个有命令行界面 (CLI) 工具的客户端。
![avatar](https://414706657.oss-cn-shenzhen.aliyuncs.com/psb.png)

--------------------- 

## Docker 系统架构

- Docker 使用客户端-服务器 (C/S) 架构模式，使用远程 API 来管理和创建 Docker 容器。
- Docker 容器通过 Docker 镜像来创建。
- 容器与镜像的关系类似于面向对象编程中的对象与类。

Docker | 面向对象 
:-: | :- 
容器 | 对象
镜像 | 类


![](https://414706657.oss-cn-shenzhen.aliyuncs.com/psb_1.png)

--------------------- 

标题 | 说明 
:-: | :- 
镜像</br>(Images)|Docker 镜像是用于创建 Docker 容器的模板。
容器</br>(Container)|容器是独立运行的一个或一组应用。
客户端</br>(Client)|Docker 客户端通过命令行或者其他工具使用 Docker API (https://docs.docker.com/reference/api/docker_remote_api) 与 Docker 的守护进程通信。
主机</br>(Host)|一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。
仓库</br>(Registry)|Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。Docker Hub(https://hub.docker.com) 提供了庞大的镜像集合供使用。
Docker Machine|Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。

--------------------- 

## Docker 镜像

Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。

#### 分层存储
- 镜像并非是像一个 ISO 那样的打包文件，镜像只是一个虚拟的概念，其实际体现并非由一个文件组成，而是由一组文件系统组成，或者说，由多层文件系统联合组成。
- 镜像构建时，会一层层构建，前一层是后一层的基础。每一层构建完就不会再发生改变，后一层上的任何改变只发生在自己这一层。构建镜像的时候，需要额外小心，每一层尽量只包含该层需要添加的东西，任何额外的东西应该在该层构建结束前清理掉。
- 分层存储的特征还使得镜像的复用、定制变的更为容易。甚至可以用之前构建好的镜像作为基础层，然后进一步添加新的层，以定制自己所需的内容，构建新的镜像。


#### 镜像操作
指令 | 说明 
:-: | :- 
docker image ls</br> docker images| 列出镜像
docker image rm	镜像ID</br>docker image 镜像名字:版本</br>docker rmi 镜像ID|删除镜像
docker image prune|删除虚悬镜像


--------------------- 

## Docker容器

#### 容器操作
指令 | 说明 
:-: | :-
docker container ls<br>docker ps|当前启动容器
docker container ls -a<br>docker ps -a | 全部容器
docker start 容器ID|启动一个容器
docker container start 容器ID | 启动已终止容器
docker container stop 容器ID|终止容器
docker container rm|来删除一个处于终止状态的容器<br>添加 -f 参数,删除一个运行中的容器
docker container prune|清理掉所有处于终止状态的容器。
docker exec -it 容器ID bash|以交互式进入容器

- 通过 exit 命令或 Ctrl+d 来退出终端时，所创建的容器立刻终止
- docker attach 命令或 docker exec 命令进入容器进行操作


--------------------- 

## Dockerfile 定制镜像

## Docker Compose
- Docker Compose 是 Docker 官方编排（Orchestration）项目之一，负责快速的部署分布式应用。

##### Docker Compose 安装与卸载
###### 二进制包
从 [官方 GitHub Release](https://github.com/docker/compose/releases) 处获取最新版本。


    curl -L https://github.com/docker/compose/releases/download/版本号/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose//设置可运行权限

###### 在<font color=#A52A2A size=3 >docker-compose.yml </font>编写 <font color=#A52A2A size=3 >/usr/local/docker/tomcat</font> 文件，这个是 Compose 使用的主模板文件。 

    version: '3.1'
    services:
      tomcat:
        restart: always
        image: tomcat
        container_name: tomcat
        ports:
          - 8080:8080

#### 常用操作
指令 | 说明 
:-: | :- 
docker-compose up | 运行 compose 项目
docker-compose down |停止 up 命令所启动的容器，并移除网络(在.yml文件目录下运行)
docker-compose logs (-f) 容器ID |查看日志(监听日志)
docker-compose restart [options] [SERVICE...]|重启项目中的服务。

#### 根据条件查询日志
docker logs 容器ID |grep '查询条件'   
docker logs 40873b5dc7f7 |grep 'Current Page'

--------------------- 
## Docker三大核心概念
Docker的三大核心概念分别是镜像、容器、仓库   
Docker运行容器前需要本地存在对应的镜像，如果本地没有对应的镜像，Docker会尝试从默认的镜像仓库下载。当然用户也可以通过配置，使用自定义的镜像仓库。

---------------------
## Docker安装

#### 卸载旧版本

    $  apt-get remove docker \
               docker-engine \
               docker.io

#### 使用脚本自动安装
在测试或开发环境中 Docker 官方为了简化安装流程，提供了一套便捷的安装脚本，Ubuntu 系统上可以使用这套脚本安装：

    $ curl -fsSL get.docker.com -o get-docker.sh
    # 可能会出现 404 错误，请移步下面的特别说明
    $ sudo sh get-docker.sh --mirror Aliyun

----------------------------
## 使用 APT 安装
- 安装必要的一些系统工具

      $  sudo apt-get update
      $  sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
    
- 安装 GPG 证书

       $  curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
    
- 写入软件源信息

       $  sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"

- 更新并安装 Docker CE

        $ sudo apt-get -y update
        $ sudo apt-get -y install docker-ce

-------------------------------------
## Docker 镜像加速器
#### Ubuntu 16.04+、Debian 8+、CentOS 7
对于使用 systemd 的系统，请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）

    {
      "registry-mirrors": [
        "https://registry.docker-cn.com"
      ]
    }

#### 重新启动服务。

    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker
--------------------------------------------
## Docker Compose 安装与卸载
#### 二进制包
在 Linux 上的也安装十分简单，从 官方 [GitHub Release](https://github.com/docker/compose/releases) 处直接下载编译好的二进制文件即可。

例如，在 Linux 64 位系统上直接下载对应的二进制包

    $ sudo curl -L https://github.com/docker/compose/releases/download/1.17.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
    $ sudo chmod +x /usr/local/bin/docker-compose

-------------------------------
## 基于 Docker 安装 Tomcat
    version: '3.1'
    services:
      tomcat:
        restart: always
        image: tomcat
        container_name: tomcat
        ports:
          - 8080:8080
        volumes:
          - /usr/local/docker/tomcat/webapps/test:/usr/local/tomcat/webapps/test
        environment:
          TZ: Asia/Shanghai

## 基于 Docker 安装 MySQL
##### MySQL5
    version: '3.1'
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
##### MySQL8
    version: '3.1'
    services:
      db:
        image: mysql
        restart: always
        environment:
          MYSQL_ROOT_PASSWORD: 123456
        command:
          --default-authentication-plugin=mysql_native_password
          --character-set-server=utf8mb4
          --collation-server=utf8mb4_general_ci
          --explicit_defaults_for_timestamp=true
          --lower_case_table_names=1
        ports:
          - 3306:3306
        volumes:
          - ./data:/var/lib/mysql
    
      adminer:
        image: adminer
        restart: always
        ports:
          - 8080:8080
------------------------------
## 基于 Docker 安装 GitLab
    version: '3'
    services:
        web:
          image: 'twang2218/gitlab-ce-zh'
          restart: always
          hostname: '47.107.245.83'
          environment:
            TZ: 'Asia/Shanghai'
            GITLAB_OMNIBUS_CONFIG: |
              external_url 'http://47.107.245.83'
              gitlab_rails['gitlab_shell_ssh_port'] = 2222
              unicorn['port'] = 8888
              nginx['listen_port'] = 80
          ports:
            - '80:80'
            - '8443:443'
            - '2222:22'
          volumes:
            - ./config:/etc/gitlab
            - ./data:/var/opt/gitlab
            - ./logs:/var/log/gitlab

- 设置管理员初始密码，这里的密码最好是 字母 + 数字 组合，并且 大于等于 8 位
- 配置完成后登录，管理员账号是 root
-----------------------------------------
## 基于 Docker 安装 Registry
    version: '3'
    services:
        web:
          image: 'twang2218/gitlab-ce-zh'
          restart: always
          hostname: '47.107.245.83'
          environment:
            TZ: 'Asia/Shanghai'
            GITLAB_OMNIBUS_CONFIG: |
              external_url 'http://47.107.245.83'
              gitlab_rails['gitlab_shell_ssh_port'] = 2222
              unicorn['port'] = 8888
              nginx['listen_port'] = 80
          ports:
            - '80:80'
            - '8443:443'
            - '2222:22'
          volumes:
            - ./config:/etc/gitlab
            - ./data:/var/opt/gitlab
            - ./logs:/var/log/gitlab

#### 配置 Docker Registry 客户端
案例使用的是 Ubuntu Server 16.04 LTS 版本，属于 systemd 系统，需要在 /etc/docker/daemon.json 中增加如下内容（如果文件不存在请新建该文件）

    {
      "registry-mirrors": [
        "https://registry.docker-cn.com"
      ],
      "insecure-registries": [
        "ip:5000"
      ]
    }

##### 之后重新启动服务。

    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker



----------------------------------------
## 基于 Docker 安装 Nexus
    version: '3.1'
    services:
      nexus:
        restart: always
        image: sonatype/nexus3
        container_name: nexus
        ports:
          - 8081:8081
        volumes:
          - /usr/local/docker/nexus/data:/nexus-data
###### 注： 启动时如果出现权限问题可以使用：chmod 777 /usr/local/docker/nexus/data 赋予数据卷目录可读可写的权限
##### 登录控制台验证安装
地址：http://ip:port/   
用户名：admin   
密码：admin123   
--------------------------------
## 基于 Docker 安装 Zookeeper
##### Zookeeper 部署有三种方式，单机模式、集群模式、伪集群模式，以下采用 Docker 的方式部署
###### 单机模式
###### docker-compose.yml
    version: '3.1'
    
    services:
        zoo1:
            image: zookeeper
            restart: always
            hostname: zoo1
            ports:
                - 2181:2181
            environment:
                ZOO_MY_ID: 1
                ZOO_SERVERS: server.1=zoo1:2888:3888


##### 集群模式
准备 3 台 Ubuntu Server 系统，并分别配置 Zookeeper

###### 第一台主机
###### docker-compose.yml
    version: '3.1'
    services:
        zoo1:
            image: zookeeper
            restart: always
            environment:
                ZOO_MY_ID: 1
                ZOO_SERVERS: server.1=192.168.75.130:2888:3888 server.2=192.168.75.134:2888:3888 server.3=192.168.75.135:2888:3888
            network_mode: host
##### 验证测试
    root@UbuntuBase:/usr/local/docker/zookeeper# docker exec -it zookeeper_zoo1_1 /bin/bash
    bash-4.3# ./bin/zkServer.sh status
    ZooKeeper JMX enabled by default
    Using config: /conf/zoo.cfg
    Mode: leader
##### 第二台主机
###### docker-compose.yml
    version: '3.1'
    services:
        zoo2:
            image: zookeeper
            restart: always
            environment:
                ZOO_MY_ID: 2
                ZOO_SERVERS: server.1=192.168.75.130:2888:3888 server.2=192.168.75.134:2888:3888 server.3=192.168.75.135:2888:3888
            network_mode: host
###### 验证测试
    root@UbuntuBase:/usr/local/docker/zookeeper# docker exec -it zookeeper_zoo2_1 /bin/bash
    bash-4.3# ./bin/zkServer.sh status
    ZooKeeper JMX enabled by default
    Using config: /conf/zoo.cfg
    Mode: follower
##### 第三台主机
###### docker-compose.yml
    version: '3.1'
    services:
        zoo3:
            image: zookeeper
            restart: always
            environment:
                ZOO_MY_ID: 3
                ZOO_SERVERS: server.1=192.168.75.130:2888:3888 server.2=192.168.75.134:2888:3888 server.3=192.168.75.135:2888:3888
            network_mode: host
##### 验证测试
    root@UbuntuBase:/usr/local/docker/zookeeper# docker exec -it zookeeper_zoo3_1 /bin/bash
    bash-4.3# ./bin/zkServer.sh status
    ZooKeeper JMX enabled by default
    Using config: /conf/zoo.cfg
    Mode: follower


##### 伪集群模式
###### docker-compose.yml
    version: '3.1'
    services:
        zoo1:
            image: zookeeper
            restart: always
            hostname: zoo1
            ports:
                - 2181:2181
            environment:
                ZOO_MY_ID: 1
                ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=zoo2:2888:3888 server.3=zoo3:2888:3888
        zoo2:
            image: zookeeper
            restart: always
            hostname: zoo2
            ports:
                - 2182:2181
            environment:
                ZOO_MY_ID: 2
                ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=zoo2:2888:3888 server.3=zoo3:2888:3888
    
        zoo3:
            image: zookeeper
            restart: always
            hostname: zoo3
            ports:
                - 2183:2181
            environment:
                ZOO_MY_ID: 3
                ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=zoo2:2888:3888 server.3=zoo3:2888:3888

##### 验证是否安装成功
###### 分别以交互方式进入容器查看
    docker exec -it zookeeper_zoo1_1 /bin/bash
    docker exec -it zookeeper_zoo2_1 /bin/bash
    docker exec -it zookeeper_zoo3_1 /bin/bash
###### 使用服务端工具检查服务器状态
    root@UbuntuBase:/usr/local/docker/zookeeper# docker exec -it zookeeper_zoo1_1 /bin/bash
    bash-4.3# ./bin/zkServer.sh status
    ZooKeeper JMX enabled by default
    Using config: /conf/zoo.cfg
    Mode: follower
    
    root@UbuntuBase:/usr/local/docker/zookeeper# docker exec -it zookeeper_zoo2_1 /bin/bash
    bash-4.3# ./bin/zkServer.sh status
    ZooKeeper JMX enabled by default
    Using config: /conf/zoo.cfg
    Mode: follower