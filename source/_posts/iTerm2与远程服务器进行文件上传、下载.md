---
title: iTerm2与远程服务器进行文件上传、下载
date: 2021-04-30 14:37:51
tags:
---
- 上传文件
```test
#scp 【本地文件路径】 【服务器用户名】@【服务器ip】：【服务器文件存放路径】
scp local_folder remote_username@remote_ip:remote_folder

#示例
scp /usr/123.txt root@192.168.31.200:/home

#上传文件夹(加-r参数)
scp -r local_folder   remote_username@remote_ip:remote_folder
```
- 下载文件
```test
#scp 【服务器用户名】@【服务器地址】：【服务器上存放文件的路径】【本地文件的路径】
scp -r remote_username@remote_ip:remote_folder local_folder
```
- scp相关参数
```test
-v 详细显示输出,显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题
-r 递归处理，递归复制整个目录
-C 使能压缩选项
-p 保留原文件的修改时间，访问时间和访问权限。
-P port 选择传输端口
-4 强行使用 IPV4 地址
-6 强行使用 IPV6 地址

```