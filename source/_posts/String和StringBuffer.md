---
title: String和StringBuffer
date: 2021-09-27 17:06:20
tags:
---

## 一、区别

> 1. String是字符串常量，底层是final类，意味着一旦赋值，不可改变。而StringBuffer和StringBuilder是字符串变量。由String创建的字符内容是不可改变的，而由StringBuffer和StringBuidler创建的字符内容是可以改变的。
> 2. StringBuffer是线程安全的，而StringBuilder是非线程安全的。StringBuilder是从JDK 5开始，为StringBuffer类补充的一个单线程的等价类。我们在使用时应优先考虑使用StringBuilder，因为它支持StringBuffer的所有操作，但是因为它不执行同步，不会有线程安全带来额外的系统消耗，所以速度更快。
>3. StringBuffer是线程安全的，因为它内部使用了synchronized修饰，所以效率比StringBuilder要弱15%。

## 二、效率对比
三者builder最快其次buffer最后是String。

