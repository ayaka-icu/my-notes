---
title: Java网络编程👻复习
date: 2021-06-28 17:36:26
tags: ["后端","Java基础","网络编程"]
cover: "https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/img/blog-top/network-top.png"
categories:
  - 后端
  - Java
  - Java基础
---





# 网络编程基础总复习





## 网络编程概念



![网络编程](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B.assets/网络编程.png)



## Java IP地址获取

InetAddress的使用

![IP](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B.assets/IP.png)

代码

```JAVA
package 基础知识.网络编程.IP地址;
/*
1.4 InetAddress的使用
为了方便我们对IP地址的获取和操作,Java提供了一个类 InetAddress供我们使用

InetAddress:此类表示 Internet协议(IP)地址

    static InetAddress getByName(String host) 确定主机名称的IP地址。主机名称可以是机器名称,也可以是IP地址

    String getHostName()       获取此IP地址的主机名

    String getHostAddress()    返回文本显示中的P地址字符串

*/

import java.net.InetAddress;
import java.net.UnknownHostException;

public class Test_InetAddress {

    public static void main(String[] args) throws UnknownHostException {

        //static InetAddress getByName(String host)
        //确定主机名称的IP地址。主机名称可以是机器名称,也可以是IP地址
        //需要异常处理
        InetAddress iaIP = InetAddress.getByName("GanGaJiang"/*" 192.168.46.201 "*/);//参数可以是IP 也可以是主机名

        //String getHostName()       获取此IP地址的主机名
        System.out.println("主机名称: " + iaIP.getHostName() + "\n");

        //String getHostAddress()    返回文本显示中的P地址字符串
        System.out.println("主机IP地址: " + iaIP.getHostAddress());

    }

}
```



## 网络通信

概述

![网络通信](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B.assets/网络通信.png)



## UDP协议

### 发信息

![TestUDP_01发送数据](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B.assets/TestUDP_01发送数据.png)



```java
package 基础知识.网络编程.网络通信.UDP协议.A_UDP学习;
/*
    UDP发送数据的步骤
        1.创建发送端的Socket对象(DatagramSocket)
        2.创建数据, 并把数据打包
        3.调用DatagramSocket对象的方法发送数据
        4.关闭发送端

*/

import java.io.IOException;
import java.net.*;

public class TestUDP_01 {

    public static void main(String[] args) throws IOException {


        //1.创建发送端的Socket对象(DatagramSocket)
        //  构造方法：DatagramSocket() 构造数据报套接字并将其绑定到本地主机上的任何可用端口。
        DatagramSocket ds = new DatagramSocket();


        //2.创建数据, 并把数据打包
        //  DatagramPacket(byte[] buf, int length, InetAddress address, int port)
        //  构造数据报包，用来将长度为 length 的包发送到指定主机上的指定端口号。
        byte[] buf = "gan,ga,jiang".getBytes();
        /*int length = buf.length;
        InetAddress address = InetAddress.getByName("192.168.46.201");
        int port = 10086;
        DatagramPacket dp = new DatagramPacket(buf,length,address,port);*/
        DatagramPacket dp = new DatagramPacket(buf,buf.length,InetAddress.getByName("192.168.94.201"),10086);


        //3.调用DatagramSocket对象的方法发送数据
        //  send(DatagramPacket p) 从此套接字发送数据报包
        ds.send(dp);//参数是 DatagramPacket


        //4.关闭发送端
        //  close() 关闭此数据报套接字。
        ds.close();


    }

}
```



### 接收信息

![TestUDP_02接收数据](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B.assets/TestUDP_02接收数据.png)



```java
package 基础知识.网络编程.网络通信.UDP协议.A_UDP学习;
/*
    UDP发送数据的步骤
        1:创建发送端的Socket对象(DatagramSocket)
        2:创建一个数据包, 用于接收数据
        3:调用DatagramSocket对象的方法接收数据
        4:解析数据包, 并把数据在控制台显示
        5:关闭发送端

*/

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class TestUDP_02 {

    public static void main(String[] args) throws IOException {


        //1:创建发送端的Socket对象(DatagramSocket)
        //  构造方法:DatagramSocket(int port)构造数据报套接字并将其绑定到本地主机上的指定端口。
        DatagramSocket ds = new DatagramSocket(10086);

        //2:创建一个数据包, 用于接收数据
        //  构造方法:DatagramPacket(byte[] buf, int length)构造一个 DatagramPacket用于接收长度的数据包 length 。
        byte[] bys = new byte[1024];
        DatagramPacket dp = new DatagramPacket(bys, bys.length);


        //3:调用DatagramSocket对象的方法接收数据
        //  receive(DatagramPacket p)从此套接字接收数据报包。
        ds.receive(dp);


        //4:解析数据包, 并把数据在控制台显示
        //  byte[] getData() 返回数据缓冲区。
        //  int    getLength() 返回要发送的数据的长度或接收到的数据的长度。

        byte[] datas = dp.getData();
        int len = dp.getLength();
        String dataString = new String(datas,0,len);
        System.out.println(dataString);

        //简写
        System.out.println(new String(dp.getData(),0,dp.getLength()));


        //关闭发送端
        ds.close();
    }

}
```



## UDP发送接收数据综合

### 发送数据

```java
package 基础知识.网络编程.网络通信.UDP协议.UDP发送接收数据复习;

/*
    UDP发送
*/

import java.io.*;
import java.net.*;


public class SendUDP {

    public static void main(String[] args){


        //录入需要发送的数据
        try (   //封装键盘录入功能
                BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
                //创建DatagramSocket对象
                DatagramSocket ds = new DatagramSocket();
        ) {
            //数据
            String data;
            while ((data = br.readLine()) != null){

                //创建数据报包 DatagramPacket()对象
                DatagramPacket dp = new DatagramPacket(data.getBytes(),data.getBytes().length, InetAddress.getByName("GanGaJiang"),10086);

                //发送数据报包
                ds.send(dp);

                if ( "end".equals(data) ){
                    System.out.println("已关闭发送端。");
                    return;
                }

            }

            //释放资源 在try()中已经完成

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

}
```



### 接收数据

```java
package 基础知识.网络编程.网络通信.UDP协议.UDP发送接收数据复习;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class ReceiveUDP {

    public static void main(String[] args) {

        //创建接收DatagramSocket(端口号)数据对象
        try (   DatagramSocket ds = new DatagramSocket(10086);
        ){
            //接收到的数据
            String dataString = null;
            while (true){

                //创建数据包 用于接收数据包 DatagramPacket(数据,数据长度);
                byte[] bys = new byte[1024];
                DatagramPacket dp = new DatagramPacket(bys, bys.length);

                //接收数据包
                ds.receive(dp);

                //解析数据包
                byte[] datas = dp.getData();//获取数据
                int len = dp.getLength();//获取数据长度
                dataString = new String(datas, 0, len);
                System.out.println(dataString);

                if ("end".equals(dataString)){
                    System.out.println("对方以关闭,接收端正在关闭");
                    for (int i = 0; i < 15; i++) {
                        try {
                            Thread.sleep(100);
                            System.out.print(".");
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    System.out.println("\n已关闭接收端。");

                    return;
                }
            }


        }catch (IOException e){
            e.printStackTrace();
        }


    }

}
```





## TCP协议

概述

![TCP通信原理](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B.assets/TCP通信原理.png)



### TCP发送数据

```java
package 基础知识.网络编程.网络通信.TCP协议.A_TCP学习;
/*
    TCP发送数据的步骤
        1:创建客户端的Socked对象
        2:获取输出流, 写数据
        3:释放资源

*/

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;
/*

    客户端: 发送数据, 接收服务器反馈

*/

public class Test_SendTCP {

    public static void main(String[] args) throws IOException {

        //1:创建客户端的Socked对象
        //  Socket(InetAddress address, int port) 创建流套接字并将其连接到指定IP地址的指定端口号。
        Socket sk = new Socket(InetAddress.getByName("GanGaJiang"), 10000);

        //2:获取输出流, 写数据
        //  OutputStream getOutputStream() B返回此套接字的输出流。
        OutputStream os = sk.getOutputStream();
        os.write("这就尴尬了".getBytes());

        //2.5: 接收服务器反馈
        InputStream is = sk.getInputStream();
        byte[] bys = new byte[1024];
        int len = is.read(bys);
        String data = new String(bys, 0, len);
        System.out.println("客户端:" + data);

        //3:释放资源
        //os.close();  //这两个不需要释放资源，因为os/is 对象是根据sk获取的 sk释放资源 这两个也会释放资源。
        //is.close();  //这两个不需要释放资源，因为os/is 对象是根据sk获取的 sk释放资源 这两个也会释放资源。
        sk.close();

        /*运行结果:
            Exception in thread "main" java.net.ConnectException: Connection refused: connect
          程序报错！ 报错类:ConnectException
                         --指示尝试将套接字连接到远程地址和端口时发生错误。通常，连接被远程拒绝（例如，没有进程正在监听远程地址/端口）。

          原因：TCP是三次握手的 握手失败 还没创建接收端！
        */
    }
}
```





### TCP接收数据

```java
package 基础知识.网络编程.网络通信.TCP协议.A_TCP学习;
/*
ServerSocket:
    这个类实现了服务器套接字。服务器套接字等待通过网络进入的请求。它根据该请求执行一些操作，然后可能将结果返回给请求者。
*/


import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;
/*
    服务器: 接收数据, 给出反馈
*/

public class Test_ReceiveTCP {

    public static void main(String[] args) throws IOException {

        //创建服务端的Socket对象(ServerSocket)
        ServerSocket ss = new ServerSocket(/*端口号*/10000);

        //监听客户端连接, 返回一个Socket对象
        // Socket accept() 侦听并接受到此套接字的连接。
        Socket accept = ss.accept();

        //获取输入流, 读数据, 并把数据显示在控制台
        InputStream is = accept.getInputStream();
        byte[] bys = new byte[1024];
        int len = is.read(bys);//读取数据 获取数据长度
        String datas = new String(bys, 0, len);//获取接收数据，字符串行
        System.out.println("服务器接收到数据:" + datas);


        //给出反馈
        OutputStream os = accept.getOutputStream();
        os.write("服务器接收到数据".getBytes());
        //就是io流写入数据[byte数组] 写入反馈。

        //释放资源
        ss.close();
    }
}
```



## TCP发送接收数据综合

### 发送多条信息

#### 客户端

```java
package 基础知识.网络编程.网络通信.TCP协议.TCP发送接收数据复习1;

import java.io.*;
import java.net.Socket;

/*
    客户端:
*/
public class SendTCP {

    public static void main(String[] args) {


        try (   //创建客户端Socket对象
                Socket s = new Socket("GanGaJiang", 10002);
                //获取io对象,封装键盘录入
                BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
                //将s.getOutputStream()封装成 字符缓存输出流
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
                //接收数据反馈 同样封装成BufferedReader
                BufferedReader brx = new BufferedReader(new InputStreamReader(s.getInputStream()));
        ) {

            //接收服务器反馈
            System.out.println(brx.readLine());

            //发送内容======================
            String line;
            while ((line = br.readLine()) != null) {
                if ("886".equals(line)) {
                    break;
                }
                bw.write(line);
                bw.newLine();
                bw.flush();
            }


            //已自动释放资源。
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 服务端

```java
package 基础知识.网络编程.网络通信.TCP协议.TCP发送接收数据复习1;

/*
    服务器
*/

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class ReceiveTCP {

    public static void main(String[] args) throws IOException {
        //创建服务器Socket对象
        ServerSocket ss = new ServerSocket(10002);

        try (
                //监听客户端连接, 返回一个Socket对象
                Socket s = ss.accept();
                //获取io流,读数据,封装成BufferedReader
                BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
                //创建io流, 反馈用户信息, 无法封装
                BufferedWriter bwx = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
        ) {

            //给用户链接反馈
            bwx.write("服务器接收到链接请求");
            bwx.newLine();
            bwx.flush();

            //读取客户端发送来的内容 并打印出来
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
        ss.close();
    }
}
```



#### 遇到的问题与解决

![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B.assets/img-16348083717401.png)





### 发送文件	//	接收文件

#### 客户端

```java
package 基础知识.网络编程.网络通信.TCP协议.TCP发送接收数据复习2;

import java.io.*;
import java.net.Socket;

/*
    客户端:
            客户端发文件
*/
public class SendTCP {

    public static void main(String[] args) {

        try (   //创建客户端Socket对象
                Socket s = new Socket("GanGaJiang", 10001);
                //获取io对象,读取发送文件内容
                BufferedReader br = new BufferedReader(new FileReader(new File("A1_Java基础SE\\src\\基础知识\\网络编程\\网络通信\\TCP协议\\TCP发送接收数据复习2\\SendTCP.java")));
                //将s.getOutputStream()封装成 字符缓存输出流
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
                //接收数据反馈 同样封装成BufferedReader
                BufferedReader brx = new BufferedReader(new InputStreamReader(s.getInputStream()))
        ) {

            //发送内容==================
            String line;
            while ((line = br.readLine()) != null) {
                bw.write(line);
                bw.newLine();
                bw.flush();
            }
            //直接使用 该方法正常停止标语
            s.shutdownOutput();
            /*//自定义接收标语
            bw.write("!@#$%end");
            bw.newLine();//newLine不能少！!!
            bw.flush();*/

            //接收服务器反馈
            System.out.println(brx.readLine());

            //已自动释放资源。
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



#### 服务端

```java
package 基础知识.网络编程.网络通信.TCP协议.TCP发送接收数据复习2;

/*
    服务器
        这次服务器将接受到的数据 写成一个文本

*/

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class ReceiveTCP {

    public static void main(String[] args) throws IOException {
        //创建服务器Socket对象
        ServerSocket ss = new ServerSocket(10001);

        try (
                //监听客户端连接, 返回一个Socket对象
                Socket s = ss.accept();
                //获取io流,读数据,封装成BufferedReader
                BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));

                //创建io流,将读取到的数据写入成 新文件
                BufferedWriter bw = new BufferedWriter(new FileWriter(new File("A1_Java基础SE\\src\\基础知识\\网络编程\\网络通信\\TCP协议\\TCP发送接收数据复习2\\接收文件.txt")));

                //创建io流, 反馈用户信息, 无法封装
                /*OutputStream os = s.getOutputStream();*/
                BufferedWriter bwx = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
        ) {

            //读取客户端发送来的内容
            String line;
            while ((line = br.readLine()) != null) {
                /*//自定义结束标语
                if ("!@#$%end".equals(line)) {
                    break;
                }*/
                bw.write(line);
                bw.newLine();
                bw.flush();
            }

            //给用户链接反馈
            bwx.write("文件上传成功");
            bwx.newLine();
            bwx.flush();

        } catch (IOException e) {
            e.printStackTrace();
        }
        ss.close();
    }
}
```



### 多线程  服务器端

#### 服务器端

```java
package 基础知识.网络编程.网络通信.TCP协议.TCP通信服务端口多线程;
/*
    服务器端
        时刻接收用户信息
        多线程
*/

import java.io.IOException;
import java.net.IDN;
import java.net.ServerSocket;
import java.net.Socket;

public class MyServer {
    private static int id = 1;
    public static void main(String[] args) throws IOException {

        //创建服务器端Socket对象
        ServerSocket ss = new ServerSocket(10086);

        //服务器时刻开着
        while (true){
            //监听用户链接 Socket对象
            Thread t = new Thread(new MyServerReceive(ss.accept()),("用户"+id));
            t.start();//启动线程
            id ++;
        }

        //释放资源
        //ss.close();

    }
}
```



#### 实现多线程

```java
package 基础知识.网络编程.网络通信.TCP协议.TCP通信服务端口多线程;

import java.io.*;
import java.net.Socket;

public class MyServerReceive implements Runnable{
    //用户对象Socket
    private Socket s;

    public MyServerReceive(Socket s){
        this.s = s;
    }

    @Override
    public void run() {

        //接收数据   实现反馈
        try (   BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
                BufferedWriter bw =
                        new BufferedWriter(
                                new FileWriter(
                                        new File("A1_Java基础SE\\src\\基础知识\\网络编程\\网络通信\\TCP协议\\TCP通信服务端口多线程\\服务器接收文件\\"
                                                +Thread.currentThread().getName()+".txt")));
                BufferedWriter bwx = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
        ){

            //接收数据 写入到接收文件夹中
            String data;
            while ( (data = br.readLine())!=null ){
                bw.write(data);
                bw.newLine();
                bw.flush();
            }
            System.out.println("接收到:"+Thread.currentThread().getName()+"用户,文件copy成功");
            //反馈用户链接
            bwx.write("已接收到连接请求,文件上传成功！");
            bwx.newLine();
            bwx.flush();

        }catch(IOException e){
            e.printStackTrace();
        }
    }
}
```



### 多个客户端 // 多个类

#### 客户端A

```java
package 基础知识.网络编程.网络通信.TCP协议.TCP通信服务端口多线程;
/*
 *
 * 用户 A
 *
 * */
import java.io.*;
import java.net.Socket;
/*
    客户端:
            客户端发文件
*/
public class UserA {
    public static void main(String[] args) {

        try (   //创建客户端Socket对象
                Socket s = new Socket("GanGaJiang", 10086);
                //获取io对象,读取发送文件内容
                BufferedReader br = new BufferedReader(new FileReader(new File("A1_Java基础SE\\src\\基础知识\\网络编程\\网络通信\\TCP协议\\TCP通信服务端口多线程\\UserA.java")));
                //将s.getOutputStream()封装成 字符缓存输出流
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
                //接收数据反馈 同样封装成BufferedReader
                BufferedReader brx = new BufferedReader(new InputStreamReader(s.getInputStream()))
        ) {

            //发送内容==================
            String line;
            while ((line = br.readLine()) != null) {
                bw.write(line);
                bw.newLine();
                bw.flush();
            }
            //停止标语
            s.shutdownOutput();

            //接收服务器反馈
            System.out.println(brx.readLine());

            //已自动释放资源。
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 客户端B

```JAVA
package 基础知识.网络编程.网络通信.TCP协议.TCP通信服务端口多线程;
/*
 *
 * 用户 B
 *
 * */
import java.io.*;
import java.net.Socket;
/*
    客户端:
            客户端发文件
*/
public class UserB {
    public static void main(String[] args) {

        try (   //创建客户端Socket对象
                Socket s = new Socket("GanGaJiang", 10086);
                //获取io对象,读取发送文件内容
                BufferedReader br = new BufferedReader(new FileReader(new File("A1_Java基础SE\\src\\基础知识\\网络编程\\网络通信\\TCP协议\\TCP通信服务端口多线程\\UserB.java")));
                //将s.getOutputStream()封装成 字符缓存输出流
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
                //接收数据反馈 同样封装成BufferedReader
                BufferedReader brx = new BufferedReader(new InputStreamReader(s.getInputStream()))
        ) {

            //发送内容==================
            String line;
            while ((line = br.readLine()) != null) {
                bw.write(line);
                bw.newLine();
                bw.flush();
            }
            //停止标语
            s.shutdownOutput();
            //接收服务器反馈
            System.out.println(brx.readLine());
            //已自动释放资源。
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 客户端C

```JAVA
package 基础知识.网络编程.网络通信.TCP协议.TCP通信服务端口多线程;
/*
 *
 * 用户 C
 *
 * */
import java.io.*;
import java.net.Socket;
/*
    客户端:
            客户端发文件
*/
public class UserC {
    public static void main(String[] args) {

        try (   //创建客户端Socket对象
                Socket s = new Socket("GanGaJiang", 10086);
                //获取io对象,读取发送文件内容
                BufferedReader br = new BufferedReader(new FileReader(new File("A1_Java基础SE\\src\\基础知识\\网络编程\\网络通信\\TCP协议\\TCP通信服务端口多线程\\UserC.java")));
                //将s.getOutputStream()封装成 字符缓存输出流
                BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
                //接收数据反馈 同样封装成BufferedReader
                BufferedReader brx = new BufferedReader(new InputStreamReader(s.getInputStream()))
        ) {

            //发送内容==================
            String line;
            while ((line = br.readLine()) != null) {
                bw.write(line);
                bw.newLine();
                bw.flush();
            }
            //停止标语
            s.shutdownOutput();
            //接收服务器反馈
            System.out.println(brx.readLine());
            //已自动释放资源。
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```





## 单词复习

|      单词       |   翻译 // 使用   |
| :-------------: | :--------------: |
|                 |                  |
| **InetAddress** | **因特网IP地址** |
|    **Host**     |     **主机**     |
|  **Datagram**   |   **数据报包**   |
|   **Socket**    | **插座//套接字** |
|   **Packet**    |    **数据包**    |
|    **send**     |     **发送**     |
|   **receive**   |     **接收**     |
|   **Server**    |    **服务器**    |
|   **Client**    |    **客户端**    |
|   **accept**    |     **接受**     |
|  **shutdown**   |     **关闭**     |
|  **Internet**   |    **因特网**    |
|                 |                  |
|                 |                  |



