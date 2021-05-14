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

* 插件需要激活
* 支持自动热部署，如果未起作用，可能是项目自动编译没有成功，手动Ctrl+F9(建议设置为Ctrl+s) 编译项目

## RestfulToolkit

### 一套 RESTful 服务开发辅助工具集。

* Ctrl+\, 全局搜索接口便可定位相关接口位置

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_1.gif) 

## Free MyBatis plugin

### 一款增强idea对mybatis支持的插件

* 生成mapper xml文件
* mybatis自动补全及语法错误提示
* 快速从代码跳转到mapper及从mapper返回代码

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_2.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_3.png)

# 使用技巧

## IDEA常用快捷键：

* Ctrl+F：页面搜索
* Ctrl+Shift+F：全局搜索
* Ctrl+G：定位到文件某一行
* Ctrl+O：重写基类方法
* Alt+/：自动完成
* Alt+Enter：自动提示完成，抛出异常
* Ctrl+/：使用//注释
* Ctrl+Shift+/：使用/**/注释
* Ctrl+Alt+L：格式化代码
* Ctrl+Alt+I：自动缩进行
* Ctrl+Alt+O：优化import
* Shift+Enter：在当前行的下方开始新行
* Ctrl+Z：撤销
* Ctrl+Shift+Z：重做
* Ctrl+X：剪贴
* Ctrl+D: 复制当前行
* Ctrl+Shift+U：切换大小写

## IDEA如何开启并配置services窗口

* 点击菜单栏：Views -> Tool Windows -> Services；中文对应：视图 -> 工具窗口 -> 服务；快捷键是Alt + F8

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_4.png)

* 刚创建好的窗口是空白的，需要我们把服务加进去。也是比较简单：点击最右侧加号Add Service，选择Run Configuration Type，最后选择SpringBoot，IDEA就会把所有项目加进来了

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_5.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_6.png)

## IDEA取消包名折叠设置

* 找到左上角project的 show options menu按钮，打开后将 flatten packages 和 hidden empty middle packages勾选上，效果就和eclipse中的包一样了，不会折叠。看起来更方便一些。

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_7.png)

## IDEA运行多个实例

 ![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_9.png)
 ![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDEA_8.png)

## IDEA设置包数量

 ![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDIA_10.png)

 ## IDEA类和方法注释模板设置
 ![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea/IDIA_11.png)
 

``` text
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end
#parse("File Header.java")
/**
* @author: 胡文良
* @ClassName ${NAME}.java
* @description: ${description}
* @create: ${YEAR}-${MONTH}-${DAY} ${HOUR}:${MINUTE}
**/
public class ${NAME} {
}
 ```
