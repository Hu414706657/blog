---
title: 使用IDEA自带分析器优化未引用的代码及参数
date: 2021-09-27 17:10:07
tags:
---

## 代码分析步骤：

1. 点击Run Inspection by Name
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/%E4%BB%A3%E7%A0%81%E4%BC%98%E5%8C%96/screenshot-20210927-101715.png)
2. 输入undeclared，打开检查项
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/%E4%BB%A3%E7%A0%81%E4%BC%98%E5%8C%96/screenshot-20210927-101804.png)
3. 排查范围筛选
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/%E4%BB%A3%E7%A0%81%E4%BC%98%E5%8C%96/screenshot-20210927-102258.png)

## 代码分析

1. 冗余参数的优化：
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/%E4%BB%A3%E7%A0%81%E4%BC%98%E5%8C%96/screenshot-20210927-101859.png)
2. 未引用的方法优化
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/%E4%BB%A3%E7%A0%81%E4%BC%98%E5%8C%96/screenshot-20210927-103233.png)
