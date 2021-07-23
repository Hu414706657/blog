---
title: Linux之alias别名
date: 2021-07-23 17:39:13
tags:
---

### 场景说明
日常查看log日志时常常需要输入<code>cd /opt/log/... </code> 需要输入一长串命令，这时我们可以将常用的命令设置一个别名例如jumplog来代之命令

### 实践
- Linux修改.bashrc(位置：~/.bashrc1)文件，添加alias语句
- 修改完执行 **`source ~/.bashrc1`** 生效

### 语法
```text
alias 别名=’原命令 -选项/参数’

es: alias ll='ls -lt'
```
