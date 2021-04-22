---
title: Linux
date: 2019-08-20 15:52:49
tags:
---



# 目录结构
![avatar](https://414706657.oss-cn-shenzhen.aliyuncs.com/psb.jpg)

| 目录 | 说明                                                                | 
|:-: | --------------------------------------------------------------------- | 
| bin  | 存放二进制可执行文件(ls,cat,mkdir等)                       | 
| boot | 存放用于系统引导时使用的各种文件                      |
| dev  | 用于存放设备文件                                              |  
| etc  | 存放系统配置文件                                              | 
| home | 存放所有用户文件的根目录                                  | 
| lib  | 存放跟文件系统中的程序运行所需要的共享库及内核模块 | 
| mnt  | 系统管理员安装临时文件系统的安装点                   |  
| opt  | 额外安装的可选应用程序包所放置的位置                | 
| proc | 虚拟文件系统，存放当前内存的映射                      |
| root | 超级用户目录                                                    | 
| sbin | 存放二进制可执行文件，只有root才能访问               | 
| tmp  | 用于存放各种临时文件                                        |
| usr  | 用于存放系统应用程序，比较重要的目录/usr/local 本地管理员软件安装目录 |
| var  | 用于存放运行时需要改变数据的文件                      | 

--------------------- 


#### 基础知识
- . 表示当前目录
- ..表示上层目录
- .so结尾文件：C 类库
- .dll结尾文件：动态链接库  c#，c++，c
- .jar结尾文件：java 类库

--------------------- 

#### 常用快捷键

指令 | 说明
:-: | :- 
Ctrl+C |退出
Ctrl+insert|粘贴

--------------------- 
#### 常用指令

指令 | 说明
:-: | :- 
showdown -h now | 立即关机
cd  |   进入一个目录
ll  |   展示目录列表(只有管理员可用)
ls  |   展示目录列表
ls -al  |   展示全部列表(包括隐藏文件)
.   |   当前目录
..  |   上层目录
cd ~/cd |进入当前用户主目录
su username |   切换用户
sudo su/su |    普通用户切换成root用户
lsb_release -a|查看版本信息

--------------------- 

#### 操作文件目录


#### 系统管理命令
<table>
<thead>
<tr>
<th style="text-align:left">命令</th>
<th style="text-align:left">说明</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">stat</td>
<td style="text-align:left">显示指定文件的相关信息,比ls命令显示内容更多</td>
</tr>
<tr>
<td style="text-align:left">who</td>
<td style="text-align:left">显示在线登录用户</td>
</tr>
<tr>
<td style="text-align:left">hostname</td>
<td style="text-align:left">显示主机名称</td>
</tr>
<tr>
<td style="text-align:left">uname</td>
<td style="text-align:left">显示系统信息</td>
</tr>
<tr>
<td style="text-align:left">top</td>
<td style="text-align:left">显示当前系统中耗费资源最多的进程</td>
</tr>
<tr>
<td style="text-align:left">ps</td>
<td style="text-align:left">显示瞬间的进程状态</td>
</tr>
<tr>
<td style="text-align:left">du</td>
<td style="text-align:left">显示指定的文件（目录）已使用的磁盘空间的总量</td>
</tr>
<tr>
<td style="text-align:left">df</td>
<td style="text-align:left">显示文件系统磁盘空间的使用情况</td>
</tr>
<tr>
<td style="text-align:left">free</td>
<td style="text-align:left">显示当前内存和交换空间的使用情况</td>
</tr>
<tr>
<td style="text-align:left">ifconfig</td>
<td style="text-align:left">显示网络接口信息</td>
</tr>
<tr>
<td style="text-align:left">ping</td>
<td style="text-align:left">测试网络的连通性</td>
</tr>
<tr>
<td style="text-align:left">netstat</td>
<td style="text-align:left">显示网络状态信息</td>
</tr>
<tr>
<td style="text-align:left">clear</td>
<td style="text-align:left">清屏</td>
</tr>
<tr>
<td style="text-align:left">kill</td>
<td style="text-align:left">杀死一个进程</td>
</tr>
</tbody>
</table>

## Mysql基本操作
指令 | 说明 
:-: | :- 
quit | 数据库退出
--------------------- 

    nano /etc/ssh/sshd_config
    
    # Authentication:
    LoginGraceTime 120
    #PermitRootLogin without-password     //注释此行
    PermitRootLogin yes                             //加入此行
    StrictModes yes
    
    重启服务
    service ssh restart
    
--------------------- 

#### 重启
- reboot
- shutdown -r now   
#### 关机
- shutdown -h now
--------------------- 
#### 解压缩 -tar
- 压缩文件夹：tar -zcvf test.tar.gz test\
- 解压文件夹：tar -zxvf test.tar.gz
---------------------
## 修改数据源
#### 查看系统版本
    lsb_release -a
#### 编辑数据源
    vi /etc/apt/sources.list
删除全部内容并修改为   

    deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse

#### 更新数据源
    apt-get update
   
---------------------------------- 
#### 安装软件包
    apt-get install packagename
    
#### 删除软件包
    apt-get remove packagename
更新软件包列表
apt-get update
