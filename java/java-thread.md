---
title: Java线程与进程👻复习
date: 2021-05-30 14:36:54
tags: ["后端","Java基础","线程"]
cover: "https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/img/blog-top/thread.jpg"
categories:
  - 后端
  - Java
  - Java基础
---





# Java进程与线程总结

[TOC]

## 图解

进程与线程

![概述](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/概述.png)





## Thread类的使用

多线程的实现方式
方式1：继承Thread类

- 定义一个类MyThread继承Thread类
- 在MyThread类中重写run()方法
- 创建MyThread类的对象
- 启动线程



两个小问题：

1. 为什么要重写run()方法？

- 因为run()是用来封装被线程执行的代码

2. run0方法和start()方法的区别？

- run():封装线程执行的代码，直接调用，相当于普通方法的调用
- start():启动线程；然后由VM调用此线程的runO方法



代码

```java
package 基础知识.进程和线程.A1_Thread类;
/*

方式1: 继承Thread类
        -- 定义一个MyThread类去继承Thread类
        -- 在MyThread类中重写run()方法
        -- 创建MyThread类的对象
        -- 启动线程 start()方法
        

查看当前线程的名称 Thread.currentThread().getName()
设置线程名称 setName();
调用线程名称 getName();

默认线程名称 Thread-0 Thread-1 ...   从0开始

*/

public class RunsName {

    public static void main(String[] args) {
        //查看当前线程的名称 main
        System.out.println(Thread.currentThread().getName());

        method1();//默认线程名字
        method2();//设置线程名称


    }

    //默认线程名字
    public static void method1(){
        //创建多线程
        MyThreadName t1 = new MyThreadName();
        MyThreadName t2 = new MyThreadName();

        //默认情况下 查看名称
        t1.start(); // Thread-0 : 0 ...
        t2.start(); // Thread-1 : 0 ...
    }


    //设置线程名称
    public static void method2(){
        //创建线程
        MyThreadName tn1 = new MyThreadName();
        MyThreadName tn2 = new MyThreadName();
        //设置名字
        tn1.setName("Ayaka001")/*设置名字*/;
        tn2.setName("Ayaka002")/*设置名字*/;
        //启动线程
        tn1.start(); // Ayaka001 : 67 ...
        tn2.start(); // Ayaka002 : 66 ...
    }

}

//继承Thread类 重写run方法 调用getName方法
class MyThreadName extends Thread {

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
        for (int i = 0; i < 100; i++) {
            System.out.println(getName()/*调用名字*/ + " : " + i);
        }
    }
}
```



## 线程调度



线程调度
线程有两种调度模型

- 分时调度模型：所有线程轮流使用CPU的使用权，平均分配每个线程占用CPU的时间片

- 抢占式调度模型：优先让优先级高的线程使用CPU,如果线程的优先级相同，那么会随机选择一个，优先级高的线程
  获取的CPU时间片相对多一些

  

Java使用的是抢占式调度模型
假如计算机只有一个CPU,那么CPU在某一个时刻只能执行一条指铃，线程只有得到CPU时间片，也就是使用权，才可以执行指令。所以说多线程程序的执行是有随机性，因为谁抢到CPU的使用权是不一定的

Thread类中设置和获取线程优洗级的方法

- public final int getPriority0:返回此线程的优先级
- public final void setPriority(int newPriority):更改此线程的优先级
    线程默认优先级是5；线程优先级的范围是：1-10
    线程优先级高仅仅表示线程获取的CU时间片的几率高，但是要在次数比较多，或者多次运行的时候才能看到你想要的效果



代码

```java
// 线程名.setPriority(p)
// 静态参数:
// Thread.MIN_PRIORITY    //1
// Thread.NORM_PRIORITY   //5
// Thread.MAX_PRIORITY    //10
public class runs {

    public static void main(String[] args) {
        //创建多线程对象
        ThreadPriority tp1 = new ThreadPriority();
        ThreadPriority tp2 = new ThreadPriority();
        ThreadPriority tp3 = new ThreadPriority();

        //设置名字
        tp1.setName("高铁");  //5 默认调度是5
        tp2.setName("飞机");  //5
        tp3.setName("拖拉机");//5

        //Thread类中有三个静态成员变量
        System.out.println(Thread.MIN_PRIORITY); //1
        System.out.println(Thread.NORM_PRIORITY);//5
        System.out.println(Thread.MAX_PRIORITY); //10
        System.out.println("还可以在中间取int设置调度");

        System.out.println("==========================");

        //调度是无序的 设置了调度只是抢到CPU片的概率高了而已,并不代表一定先运行。

        //默认调度 启动多线程
//        priority_NORM(tp1);
//        priority_NORM(tp2);
//        priority_NORM(tp3);

        //设置调度
        priority_MIN_TO_MAX(tp1,5);
        priority_MIN_TO_MAX(tp2,10);
        priority_MIN_TO_MAX(tp3,1);


        //查看各线程的调度
        System.out.println(tp1.getPriority());
        System.out.println(tp2.getPriority());
        System.out.println(tp3.getPriority());

    }

    public static void priority_NORM(ThreadPriority tp){
        tp.start();
    }

    public static void priority_MIN_TO_MAX(ThreadPriority tp,int p){
        //设置线程调度
        tp.setPriority(p);
        tp.start();
    }
}

//================================================================================================
//================================================================================================

public class ThreadPriority extends Thread{
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(getName() + ": " + i);
        }
    }
}

```



## 线程控制



![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/img-16342630686282.png)



### sleep线程停留

```java
//==============================sleep==============================
package 基础知识.进程和线程.A3_线程控制;

public class Runs_sleep {

    public static void main(String[] args) {

        //创建线程
        ThreadSleep ts1 = new ThreadSleep();
        ThreadSleep ts2 = new ThreadSleep();
        ThreadSleep ts3 = new ThreadSleep();
        //设置线程名字
        ts1.setName("曹操");
        ts2.setName("刘备");
        ts3.setName("孙权");

        //启动线程
        ts1.start();
        ts2.start();
        ts3.start();
        // 刘备:0
        // 曹操:0
        // 孙权:0
        // 曹操:1
        // 刘备:1
        // 孙权:1
        // 刘备:2
        // 曹操:2
        // 孙权:2
        // 刘备:3
        // 曹操:3
        // 孙权:3
        // ...
    }

}

//线程类
class ThreadSleep extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(getName() + ":" + i);
            //使用sleep静态方法，没执行一次停留1000毫秒
            try {//需要异常处理 try...catch
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}


```

### join等待线程死亡

```java
//==============================join==============================
package 基础知识.进程和线程.A3_线程控制;

public class Runs_join {

    public static void main(String[] args) {

        //创建线程对象
        ThreadJoin tj1 = new ThreadJoin();
        ThreadJoin tj2 = new ThreadJoin();
        ThreadJoin tj3 = new ThreadJoin();
        //设置线程名字
        tj1.setName("曹操");
        tj2.setName("曹丕");
        tj3.setName("曹植");

        //启动线程
        tj1.start();

        try {//依然需要异常处理 try...catch...
            tj1.join();//等待曹操死后......
                       //三国称霸 太子夺嫡...
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        tj2.start();
        tj3.start();

    }

}

class ThreadJoin extends Thread{

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(getName() + ":" + i);
        }
    }

}
```

### setDaemon 守护线程

```java
//==============================setDaemon==============================
package 基础知识.进程和线程.A3_线程控制;

public class Runs_setDaemon {

    public static void main(String[] args) {

        //创建线程对象
        Thread_setDaemon td1 = new Thread_setDaemon();
        Thread_setDaemon td2 = new Thread_setDaemon();

        //设置线程名称
        td1.setName("关羽");
        td2.setName("张飞");

        //当大哥刘备死后，关羽刘备应该自刎! [不求同年同月同日生,但求同年同月死]
        td1.setDaemon(true);//将关羽设置为守护线程
        td2.setDaemon(true);//将张飞设置为守护线程
        //守护线程设置要写在[启动线程]的前面

        //启动线程
        td1.start();
        td2.start();

        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        //大哥刘备是主线程
        for (int i = 0; i < 10; i++) {
            System.out.println("大哥刘备:" + i);
        }
        /*
        大哥刘备:9 //大哥死后,其他关羽与张飞并不会立即死亡
        张飞:59   // 自裁也要有时间的，但随后就会死去。。。
        关羽:61
        张飞:60
        张飞:61
        张飞:62
        关羽:62
        张飞:63
        关羽:63
        张飞:64

        */
    }

}

class Thread_setDaemon extends Thread {

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(getName() + ":" + i);
        }
    }
}
```





## 线程生命周期



![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/img-16342630876483.png)





## Runnable接口 实现多线程



![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/img-16342633195665.png)



```java
package 基础知识.进程和线程.A5_Runnable接口;
/*
方式二: 实现Runnable接口
    --定义一个类MyRunnable实现Runnable接口
    --在MyRunnable类中覆盖重写run()方法
    --创建MyRunnable类的对象
    --创建Thread类的对象,把MyRunnable对象作为构造方法的参数
    --启动线程

多线程的实现方案有两种:
    第一种: 继承Thread类
    第二种: 实现Runnable接口

相比继承Thread类,实现Runnable接口的好处:
    第一: 避免了Java单继承的局限行,接口是可以继承其他的类的
    第二: 适合多个相同程序的代码去处理同一个资源的情况,把线程和程序的代码、数据有效分离,
            较好的体现了面向对象的设计事项。


*/

public class Runs {
    public static void main(String[] args) {
        //创建Runnable接口实现对象
        MyRunnable my = new MyRunnable();

        //将my作为参数传给Thread构造方法，创建Thread
        Thread t1 = new Thread(my);
        Thread t2 = new Thread(my);

        //这样是默认线程名，可以设置名字，也可以使用构造方法
        Thread t3 = new Thread(my, "Ayaka");

        //启动线程
        t1.start();
        t2.start();
        t3.start();

    }
}

//实现Runnable接口 实现run()方法
class MyRunnable implements Runnable {

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + ":" + i);
            //Runnable接口中没后设置名字的方法。
            //使用获取当前线程名字的方法获取。
        }
    }
}
```





## 线程同步

售票案例与问题

```java
//实现多线程
public class MyRunnable implements Runnable {
    private int tick = 100;//门票总数
    @Override
    public void run() {
        while (true) {
            if (tick > 0) {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + "正在出售第:" + tick + "张票");
                tick--;

            }
        }
    }
}

//启动========================================================================================
package 基础知识.进程和线程.A6_线程同步.B1_售票案例;
/*
出现的问题:
    1.出现重复的票数
    窗口1正在出售第:100张票
    窗口3正在出售第:100张票
    窗口2正在出售第:100张票

    2.票数出现负数票数
    窗口2正在出售第:-1张票

*/

public class Runs {

    public static void main(String[] args) {

        //创建Runnable实现类对象
        MyRunnable runn = new MyRunnable();

        //创建Thread对象
        Thread t1 = new Thread(runn, "窗口1");
        Thread t2 = new Thread(runn, "窗口2");
        Thread t3 = new Thread(runn, "窗口3");

        //开始售票
        t1.start();
        t2.start();
        t3.start();

    }

}

```

售票案例中的问题

![问题分析1](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/问题分析1.png)

![问题分析2](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/问题分析2.png)





解决方案 与 规范

![img1](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/img1.png)

![img2](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/img2.png)

```java
package 基础知识.进程和线程.A6_线程同步.B2_售票改进_多线程安全_同步代码块;

/*
线程安全问题：
    -- 是否是多线程环境
    -- 是否有共享数据
    -- 是否有多条语句操作共享数据

三条语句不能同时满足！！！

解决方案:
破环其中一条即可,该案例只能破环第三条,(前两项再用)

使用: [同步代码块]
锁多条语句操作共享数据, 可以使用同步代码快实现

[格式]:
        synchronized(任意对象){
            多条语句操作共享数据的代码;
        }
[同步的好处]: 解决了多线程的数据安全问题。
[同步的弊端]: 当线程很多时,因为每个线程都会去判断同步上的锁,这是很耗费资源的,无形中会降低程序的运行效率。

*/
public class MyRunnable implements Runnable {

    private int tick = 100;
    private Object obj = new Object();//线程锁对象

    @Override
    public void run() {
        while (true) {
            synchronized (obj) {//添加同步代码块 线程?
                if (tick > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + "正在出售第:" + tick + "张票");
                    tick--;
                }
            }
        }
    }
}


//================================================================================

public class Runs {

    public static void main(String[] args) {
        MyRunnable runn = new MyRunnable();

        Thread t1 = new Thread(runn, "窗口1");
        Thread t2 = new Thread(runn, "窗口2");
        Thread t3 = new Thread(runn, "窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}   
```





<hr>



## 同步方法



![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/img-16342635839147.png)





代码

```java
public class MyRunnable implements Runnable {
    private int tick = 100;
    private Object object = new Object();
    private int jia = 0;

    @Override
    public void run() {
        while (true) {
            if (jia % 2 == 0) {
                synchronized (/*object*/this) {
                    if (tick > 0) {
                        try {
                            Thread.sleep(100);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        System.out.println(Thread.currentThread().getName() + "正在出售第:" + tick + "张票");
                        tick--;

                    }
                }
            } else {
//                synchronized (object) {
//                    if (tick > 0) {
//                        try {
//                            Thread.sleep(100);
//                        } catch (InterruptedException e) {
//                            e.printStackTrace();
//                        }
//                        System.out.println(Thread.currentThread().getName() + "正在出售第:" + tick + "张票");
//                        tick--;
//                    }
//                }
                method();
            }
            jia++;
        }
    }

    //使用[同步方法]
    public synchronized void method() {
        /*
        这样会出现安全问题
        上面的锁是 obj 对象
        而这个方法的 锁 是obj

        而是this本对象
        把上面的obj 换成this验证
        */
        if (tick > 0) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "正在出售第:" + tick + "张票");
            tick--;
        }
    }
}

//=================================================================================

public class RunsMethod {

    public static void main(String[] args) {
        MyRunnable run = new MyRunnable();

        Thread t11 = new Thread(run, "窗口1");
        Thread t12 = new Thread(run, "窗口2");
        Thread t13 = new Thread(run, "窗口3");

        t11.start();
        t12.start();
        t13.start();

        try {
            Thread.sleep(1000*20);
            System.exit(0);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}

```





## 线程同步类

线程安全的类
StringBuffer

- 线程安全，可变的字符序列

- 从版本JDKS开始，被String Builder替代。通常应该使用String Builder类，因为它支持所有相同的操作，但它更快，因为它不执行同步

  

  

Vector

- 从Java2平台v1.2开始，该类改进了List接口，使其成为lava Collections Framework的成员。与新的集合实现不同，Vector被同步。如果不需要线程安全的实现，建议使用ArrayList代替Vector

Hashtable

- 该类实现了一个哈希表，它将键映射到值。任何非ul对象都可以用作键或者值
- 从Java2平台v1.2开始，该类进行了改进，实现了Map接口，使其成为ava Collections Framework的成员。与新的集合实现不同，Hashtable被同步。如果不需要线程安全的实现，建议使用HashMap代替Hashtable



代码

```java
package 基础知识.进程和线程.A7_线程同步类;
/*
    线程安全的类:
        StringBuffer
        Vector
        Hashtable

*/

import java.util.*;

public class RunsLei {

    public static void main(String[] args) {

        StringBuffer sb1 = new StringBuffer();    //synchronized方法，线程安全的
        StringBuilder sb2 = new StringBuilder();  //没有使用synchronized修饰

        Vector<String> ve = new Vector<>();          //synchronized方法，线程安全的
        ArrayList<String> list = new ArrayList<>();  //没有使用synchronized修饰

        Hashtable<String, String> ht = new Hashtable<>(); //synchronized方法，线程安全的
        HashMap<String, String> hm = new HashMap<>();     //没有使用synchronized修饰

        //==========================================================================
        /*
        一般来说,线程同步时，使用StringBuffer、Vector、Hashtable三个类来操作
        但是,Vector和Hashtable也不经常使用。
        使用,Collections 中的方法 来操作
                public static <T> List<T> synchronizedList(List<T> list)
                public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m)
                ......
        */

        List<String> array = Collections.synchronizedList(new ArrayList<String>());
        //array就是同步安全的了
        Map<String, String> map = Collections.synchronizedMap(new HashMap<String, String>());
        //map就是同步安全的了


    }

}
```



## Lock锁



Lock锁
虽然我们可以理解同步代码块和同步方法的锁对象问题，但是我们并没有直接看到在哪里加上了锁，在哪里释放了锁，
为了更清晰的表达如何加锁和释放锁，JDK5以后提供了一个新的璥对象Lock



Lock实现提供比使用synchronized方法和语句可以获得更广泛的锁定操作
Lock中提供了获得锁和释放锁的方法

- void locko():获得锁
- void unlock():释放锁



Lock是接口不能直接实例化，这里采用它的实现类ReentrantLock来实例化
ReentrantLock的构造方法

- ReentrantLockO:创建一个ReentrantLock的实例



代码

```java
package 基础知识.进程和线程.A8_Lock锁;
/*
使用 ReentrantLock来实现Lock接口
使用 lock()方法   上锁
使用 unlock()方法 开锁

使用 try{
        lock();

    }finally{
        unlock();
    }
    代码块 进行加锁开锁 保证一定要开锁

*/

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class MyRunnable implements Runnable {
    private int tick = 100;
    //创建Lock锁
    private Lock lock = new ReentrantLock();

    //重写run方法
    @Override
    public void run() {
        while (true) {

            /*
            //上锁
            lock.lock();//上锁
            if (tick > 0) {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + ":正在出售第" + tick + "张票");
                tick--;
            }
            //开锁
            lock.unlock();//开锁
            */

            //上锁
            try {//如果这里面出现了问题,也不会影响finally里面的开锁
                lock.lock();//上锁
                if (tick > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + ":正在出售第" + tick + "张票");
                    tick--;
                }
            }finally{//finally一定执行
                //开锁
                lock.unlock();//开锁
            }

        }
    }
}

//==============================================================================
public class TestLock {
    public static void main(String[] args) {
        //创建实现Runnable接口实现类对象
        MyRunnable mr = new MyRunnable();
        //创建多线程 mr为参数
        Thread t1 = new Thread(mr,"窗口1");
        Thread t2 = new Thread(mr,"窗口2");
        Thread t3 = new Thread(mr,"窗口3");
        //启动多线程
        t1.start();
        t2.start();
        t3.start();
    }
}

```



## wait // notify



![img1](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/img1-16345477303181.png)

![img2](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/img2-16345477365022.png)

生产者消费者案例

![生产者消费者案例](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B.assets/生产者消费者案例.png)



代码启动

```java
public class BoxDame {

    //启动
    public static void main(String[] args) {
        //创建奶箱
        Box box = new Box();
        //创建生产者
        生产者 s = new 生产者(box);
        //创建消费类
        消费者 x = new 消费者(box);
        //创建多线程
        Thread ts = new Thread(s,"生产者");
        Thread tx = new Thread(x,"消费者");
        //启动多线程
        ts.start();
        tx.start();
    }
}

//=========================================================================


```

Box 奶箱类

```java
package 基础知识.进程和线程.A9_生产者和消费者.生产者消费者案例;
/*
为了体现生产者和消费的过程等待和唤醒

方法：
    void wait() 导致当前线程等待, 直到另一个线程调用该对象的notify()方法或notifyAll()方法

    void notify()  唤醒正在等待对象监视器的单线程

    void notifyAll()  唤醒正在等待对象监视器的所有线程


异常：
    IllegalMonitorStateException:
    抛出的异常表明某一线程已经试图等待对象的监视器，或者试图通知其他正在等待对象的监视器而本身没有指定监视器的线程。

调试:
    没有加 wait()方法 等待线程:
                消费者一直在去第5瓶牛奶，线程不安全

    加上后 没有加synchronized同步方法:
                出现IllegalMonitorStateException异常,

    加上后 没有加notify()方法 唤醒线程:
                运行结果: 送奶工将第1放入了奶箱  /r/n 消费者获取了第1瓶牛奶


*/

public class Box {
    //定义成员变量，牛奶的数量
    private int make;

    //定义成员变量，奶箱中是否有牛奶
    private boolean isMake = false;//默认为false

    //定义取牛奶的方法
    public /*需要同步方法*/synchronized void put(int make) {
        //添加wait() 等待线程
        //---判断如果有牛奶，就进行线程等待，等待消费者取走牛奶。
        if (isMake) {
            try {
                wait();//需要进行异常处理
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        //---等待奶箱中没有牛奶，就将生成的奶放入奶箱
        //生产牛奶后放入奶箱
        this.make = make;
        System.out.println("送奶工将第" + make + "放入了奶箱");
        //---同时设置奶箱信息 有牛奶
        this.isMake = true;

        //最后 唤醒其他等待线程
        notifyAll();
    }

    //定义获取牛奶的方法
    public /*需要同步方法*/synchronized void get() {
        //wait() 等待线程
        //---如果奶箱中没有牛奶
        if (!isMake) {
            //就打开生产者的线程，生产者放入奶箱
            try {
                wait();//需要处理异常
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        //---如果有牛奶，消费者取牛奶
        System.out.println("消费者获取了第" + make + "瓶牛奶\n");
        //---同时，设置奶箱信息 无牛奶
        this.isMake = false;

        //最后 唤醒其他等待线程
        notifyAll();
    }


}
```

生产者

```java
package 基础知识.进程和线程.A9_生产者和消费者.生产者消费者案例;

public class 生产者 implements Runnable{
    //生成奶箱
    private Box box;

    public 生产者(Box box) {
        this.box = box;
    }

    @Override
    public void run() {
        //生产者生成牛奶
        for (int i = 1; i <= 30 ; i++) {
            box.put(i);
        }
    }
}
```

消费者

```java
package 基础知识.进程和线程.A9_生产者和消费者.生产者消费者案例;

public class 消费者 implements Runnable{
    //生成奶箱
    private Box box;

    public 消费者(Box box) {
        this.box = box;
    }

    @Override
    public void run() {
        //获取牛奶
        while (true){
            box.get();
        }
    }
}
```



<hr>



## Thread 与 Runnable

比较

|  多线程   |          实现方法           |
| :-------: | :-------------------------: |
| 实现方式1 |    继承Thread重写run方法    |
| 实现方式2 | 实现Runnable接口重写run方法 |



`````java
/*
相比继承Thread类,实现Runnable接口的好处:
    第一: 避免了Java单继承的局限行,接口是可以继承其他的类的
    第二: 适合多个相同程序的代码去处理同一个资源的情况,把线程和程序的代码、数据有效分离,
            较好的体现了面向对象的设计事项。
*/
`````





## 单词复习



|       单词        |        解释         |
| :---------------: | :-----------------: |
|    **Thread**     |      **线程**       |
|     **start**     |      **起始**       |
|  **getPriority**  |  **获取线程调度**   |
|  **setPriority**  |  **设置线程调度**   |
|     **sleep**     |    **休眠 暂停**    |
|     **join**      |  **等待线程死亡**   |
|   **setDaemon**   |  **设置守护 线程**  |
|   **Runnable**    |    **/线程接口**    |
| **synchronized**  |     **同步化**      |
|    **Vector**     | **List集合 同步化** |
|   **Hashtable**   | **Map集合  同步化** |
|     **Lock**      |  **锁       接口**  |
| **ReentrantLock** | **重入锁    实现**  |
|     **lock**      |      **上锁**       |
|    **unlock**     |      **开锁**       |
|     **wait**      |      **等待**       |
|    **notify**     | **通知 /唤醒等待**  |



````java
/*

Thread				线程
start				起始
getPriority          获取线程调度
setPriority      	 设置线程调度
sleep                休眠 暂停
join				等待线程死亡
setDaemon			设置守护 线程
Runnable			/线程接口
synchronized         同步化
Vector				List集合 同步化
Hashtable            Map集合  同步化
Lock                 锁       接口
ReentrantLock        重入锁    实现
lock     		    上锁
unlock               开锁
wait                 等待
notify               通知 /唤醒等待

*/
````


