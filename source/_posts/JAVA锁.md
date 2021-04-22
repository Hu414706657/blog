---
title: JAVA锁
date: 2019-08-16 10:47:51
tags:
    - JAVA
---

# 概述

Java提供了种类丰富的锁，每种锁因其特性的不同，在适当的场景下能够展现出非常高的效率

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/lock-1.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/lock-2.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/lock-3.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/lock-4.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/lock-5.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/lock-6.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/lock-7.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/lock-8.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/lock-9.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/lock-10AQS.png)

# 线程同步
 - 使用synchronized关键字 
 - wait和notify
    - wait():使一个线程处于等待状态，并且释放所持有的对象的lock。
    - sleep():使一个正在运行的线程处于睡眠状态，是一个静态方法，调用此方法要捕捉InterruptedException异常。
    - notify():唤醒一个处于等待状态的线程，注意的是在调用此方法的时候，并不能确切的唤醒某一个等待状态的线程，而是由JVM确定唤醒哪个线程，而且不是按优先级。
    - Allnotity():唤醒所有处入等待状态的线程，注意并不是给所有唤醒线程一个对象的锁，而是让它们竞争。
 - 使用特殊域变量volatile实现线程同步
 - 使用重入锁实现线程同步
 - 使用阻塞队列实现线程同步