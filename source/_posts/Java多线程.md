---
title: java多线程
date: 2019-08-07 10:14:04
tags:
    - java
    - 多线程
---

# 进程和线程

一个程序就是一个进程，而一个程序中的多个任务则被称为线程。

进程是表示资源分配的基本单位，线程是进程中执行运算的最小单位，亦是调度运行的基本单位

在java中要想实现多线程，有两种手段，一种是继续Thread类，另外一种是实现Runable接口.(其实准确来讲，应该有三种，还有一种是实现Callable接口，并与Future、线程池结合使用

# 一、扩展java.lang.Thread类
**实例**



```java
/**
 * @Author: 胡文良
 * @Date: 2019/8/7 9:40
 */

public class ThreadDemo extends Thread {
    public static void main(String[] args) {
        Thread1 mTh1 = new Thread1("A");
        Thread1 mTh2 = new Thread1("B");
        mTh1.start();
        mTh2.start();
    }
}

class Thread1 extends Thread {
    private String name;

    public Thread1(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(name + "运行  :  " + i);
            try {
                sleep((int) Math.random() * 10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

    }
}


```

**说明**
程序启动运行main时候，java虚拟机启动一个进程，主线程main在main()调用时候被创建。随着调用mTh的两个对象的`start`方法，另外两个线程也启动了，这样，整个应用就在多线程下运行。

`start()`方法的调用后并不是立即执行多线程代码，而是使得该线程变为可运行态（Runnable），什么时候运行是由操作系统决定的。多线程程序是乱序执行。因此，只有乱序执行的代码才有必要设计为多线程。`Thread.sleep()`方法调用目的是不让当前线程独自霸占该进程所获取的CPU资源，以留出一定时间给其他线程执行的机会。实际上所有的多线程代码执行顺序都是不确定的，每次执行的结果都是随机的。

# 二、实现java.lang.Runnable接口
**实例**



```java
/**
 * @Author: 胡文良
 * @Date: 2019/8/7 9:53
 */
public class RunnableDemo {
    public static void main(String[] args) {
        new Thread(new Thread2("C", "18")).start();
        new Thread(new Thread2("D", "19")).start();
    }
}

class Thread2 implements Runnable {

    private String name;

    private String sex;

    public Thread2(String name, String sex) {
        this.name = name;
        this.sex = sex;

    }

    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(name + "运行  :  " + i);
            try {
                Thread.sleep((int) Math.random() * 10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

**说明**

Thread2类通过实现Runnable接口，使得该类有了多线程类的特征。`run（）`方法是多线程程序的一个约定。所有的多线程代码都在run方法里面。Thread类实际上也是实现了Runnable接口的类。   

在启动的多线程的时候，需要先通过Thread类的构造方法Thread(Runnable target) 构造出对象，然后调用Thread对象的start()方法来运行多线程代码。

实际上所有的多线程代码都是通过运行Thread的start()方法来运行的。因此，不管是扩展Thread类还是实现Runnable接口来实现多线程，最终还是通过Thread的对象的API来控制线程的，熟悉Thread类的API是进行多线程编程的基础

# 三、Thread和Runnable的区别

如果一个类继承Thread，则不适合资源共享。但是如果实现了Runable接口的话，则很容易的实现资源共享。

**总结：**

实现Runnable接口比继承Thread类所具有的优势：

1）：适合多个相同的程序代码的线程去处理同一个资源

2）：可以避免java中的单继承的限制

3）：增加程序的健壮性，代码可以被多个线程共享，代码和数据独立

4）：线程池只能放入实现Runable或callable类线程，不能直接放入继承Thread的类

 

提醒一下大家：main方法其实也是一个线程。在java中所以的线程都是同时启动的，至于什么时候，哪个先执行，完全看谁先得到CPU的资源。

在java中，每次程序运行至少启动2个线程。一个是main线程，一个是垃圾收集线程。因为每当使用java命令执行一个类的时候，实际上都会启动一个ＪＶＭ，每一个ｊＶＭ实习在就是在操作系统中启动了一个进程。

# 四、线程状态转换

## 一个线程的生命周期
![](https://www.runoob.com/wp-content/uploads/2014/01/java-thread.jpg)

1、新建状态（New）：新创建了一个线程对象。
2、就绪状态（Runnable）：线程对象创建后，其他线程调用了该对象的start()方法。该状态的线程位于可运行线程池中，变得可运行，等待获取CPU的使用权。
3、运行状态（Running）：就绪状态的线程获取了CPU，执行程序代码。
4、阻塞状态（Blocked）：阻塞状态是线程因为某种原因放弃CPU使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。阻塞的情况分三种：
（一）、等待阻塞：运行的线程执行wait()方法，JVM会把该线程放入等待池中。(wait会释放持有的锁)
（二）、同步阻塞：运行的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池中。
（三）、其他阻塞：运行的线程执行sleep()或join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。（注意,sleep是不会释放持有的锁）
5、死亡状态（Dead）：线程执行完了或者因异常退出了run()

# 五、多线程方法

**5.1、currentThread()方法**

currentThread()方法可返回代码段正在被哪个线程调用的信息。

Thread.currentThread().getName()

**5.2、isAlive()方法**

方法isAlive()的功能是判断当前的线程是否处于活动状态。

thread.isAlive();

**5.3、sleep()方法**

方法sleep()的作用是在指定的毫秒数内让当前"正在执行的线程"休眠（暂停执行）。这个"正在执行的线程"是指this.currentThread()返回的线程。

Thread.sleep()

**5.4、getId()方法**

getId()方法的作用是取得线程的唯一标识。

thread.getId()

