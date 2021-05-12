---
title: IDEA插件推荐及使用技巧
date: 2021-05-12 14:25:05
tags:
---

# 插件推荐

## Chinese ​(Simplified)​ Language Pack / 中文语言包 

* IDEA中文汉化插件

## JRebel and XRebel for IntelliJ
### 一款热部署插件
- 插件需要激活
- 支持自动热部署，如果未起作用，可能是项目自动编译没有成功，手动Ctrl+F9(建议设置为Ctrl+s) 编译项目

## RestfulToolkit
### 一套 RESTful 服务开发辅助工具集。
- Ctrl+\,全局搜索接口便可定位相关接口位置
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_1.gif) 

## Free MyBatis plugin
### 一款增强idea对mybatis支持的插件
- 生成mapper xml文件
- mybatis自动补全及语法错误提示
- 快速从代码跳转到mapper及从mapper返回代码
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_2.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_3.png)




# 使用技巧

## IDEA如何开启并配置services窗口
- 点击菜单栏：Views -> Tool Windows -> Services；中文对应：视图 -> 工具窗口 -> 服务；快捷键是Alt + F8
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_4.png)

- 刚创建好的窗口是空白的，需要我们把服务加进去。也是比较简单：点击最右侧加号Add Service，选择Run Configuration Type，最后选择SpringBoot，IDEA就会把所有项目加进来了
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_5.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_6.png)

## IDEA取消包名折叠设置
- 找到左上角project的 show options menu按钮，打开后将 flatten packages 和 hidden empty middle packages勾选上，效果就和eclipse中的包一样了，不会折叠。看起来更方便一些。
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_7.png)

## IDEA运行多个实例
 ![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_9.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_8.png)