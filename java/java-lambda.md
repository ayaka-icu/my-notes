---
title: Lambda表达式👻复习
date: 2021-06-06 15:10:36
tags: ["后端","Java基础","Lambda表达式"]
cover: "https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/img/blog-top/lambda.jpg"
categories:
  - 后端
  - Java
  - Java基础
---



## 📌λ表达式🌸函数式接口复习

 ![Lambda表达式](
 https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F.assets/Lambda.jpg)

在数学中，函数就是有输入量、输出量的一套计算方案，也就是“拿数据做操作”
面向对象思想强调“必须通过对象的形式来做事情”
函数式思想则尽量忽略面向对像的复杂语法：“强调做什么，而不是以什么形式去做”
而我们要学习的Lambda表达式就是函数式思想的体现



## λ表达式🍥格式

体验Lambda表达式

需求：

- 启动一个线程，在控制台输出一句话：多线程程序启动了

方式1：

- 定义一个类MyRunnable实现Runnable接口，重写runO方法
- 创建MyRunnable类的对象
- 创建Thread类的对象，把MyRunnable的对象作为构造参数传递
- 启动线程

方式2：

- 匿名内部类的方式改进

方式3：

- Lambda表达式的方式改进



![Lambda表达式格式](
https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F.assets/Lambda表达式格式.png)

代码

```java
public class TestLambda {
    public static void main(String[] args) {
        //方式一:
        RunnableImpl runImpl = new RunnableImpl();
        Thread t = new Thread(runImpl);
        t.start();

        //方式二: 通过匿名内部类改进
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("方式二: 多线程程序启动！");
            }
        }).start();

        //方式三: 通过Lambda表达式改进
        new Thread(() -> {
            System.out.println("方式三: 多线程程序启动！");
        }).start();
    }
}

public class RunnableImpl implements Runnable {
    @Override
    public void run() {
        System.out.println("方式一: 多线程程序启动！");
    }
}

```





## λ表达式🍥使用前提


Lambda表达式的使用前提

- 有一个接口
- 接口中有且仅有一个抽象方法

练习1：

- 定义一个接口(Eatable),里面定义一个抽象方法：void eat():
- 定义一个测试类(EatableDemo),在测试类中提供两个方法
  - 一个方法是：useEatable(Eatable e)
  - 一个方法是主方法，在主方法中调用useEatable方法

代码

```java
package 基础知识.Lambda表达式.A3_Lambda使用前提;

public class RunLambda {

    public static void main(String[] args) {

        String str1 = "使用匿名内部类输出";
        String str2 = "使用Lambda表达式输出";
        int len1 = 666;

        //使用匿名内部类
        me(new MyInterface() {
            @Override
            public int method(String str, int lendX, int lendY) {
                System.out.print(str + "\t");
                System.out.println(lendX + lendY);//求和
                return lendX + lendY;
            }
        }, str1, len1);

        System.out.println("=========================");

        //使用Lambda表达式
        me((/*String*/ str,/*int*/ meLen,/*int*/ lenY) -> {
            //参数类型是可以省略的,但是如果多个参数,不能只省略一个,要省略就要省略完！
            System.out.print(str + "\t");
            System.out.println(meLen + lenY);//求和
            System.out.println("Lambda表达式更为简洁！");
            return meLen + lenY;
        }, str2, len1);
    }

    //该方法有一个参数是接口，该接口又只有一个抽象方法！
    public static void me(MyInterface e, String str, int len2) {
        //该方法调用的抽象方法。
        int num = e.method(str, len2, 6000);

        //测试返回值。
        System.out.println("返回值:" + num);
    }
}
//========================================================================================

public interface MyInterface {
    //Lambda使用前提是
    //1.是个接口
    //2.接口中仅有一个抽象方法
    public abstract int method(String str, int lenX, int lenY);
}

```





<hr>





## λ表达式 优化&&特殊情况


Lambda表达式的省略规则：

- 参数类型可以省略。但是有多个参数的情况下，不能只省略一个
- 如果参数有且仅有一个，那么小括号可以省略
- 如果代码块的语句只有一条，可以省略大括号和分号，甚至是retur

代码

```java
package 基础知识.Lambda表达式.A4_特殊情况的Lambda表达式;

public class RunLambda {

    //Lambda表达式遇到特殊的接口或方法
    public static void main(String[] args) {

        //使用Lambda表达式
        me((String ssss) -> {
            System.out.println(ssss + 4);
        });

        //可以省略参数类型
        me((sss) -> {
            System.out.println(sss + 3);
        });

        //如果参数列表只有一个, 还可以直接省略 "()" 直接 参数->{ }
        me(ss -> {
            System.out.println(ss + 2);
        });

        //如果{代码块} 中 只有一条语句,还可以省略 该语句的";"  和 "{"   "}"
        me(s -> System.out.println(s + 1));

        //================================================
        //前提1: 如果{代码块} 中 只有一条语句,还可以省略 该语句的";"  和 "{"   "}"
        //前提2: 这条语句是个返回值的话, return也要省略, 不省略不行
        System.out.println( rt( (x,y)-> x + y )  /*省略了return*/  );

    }

    private static void me(MyInterface impl) {
        impl.method("这就尴尬了");
    }

    private static int rt(RutInterface impl){
        return impl.method(666, 666000);
    }
    
}

//=========================================================================================
public interface MyInterface {
    void method(String str);
}
//=========================================================================================
public interface RutInterface {
    int method(int x,int y);
}
```





<hr>




## λ表达式 注意事项

Lambda表达式的注意事项：

- 使用Lambda必须腰有接口，并且要求接口中有且仅有一个抽象方法
- 必须有上下文环境，才能推导出Lambda对应的接口
  - 根据{% label 局部变量的赋值 red %}得知Lambda对应的接口：`Runnable r = () -> System.out.println("Lambda表达式")；`
  - 根据{% label 调用方法的参数 red %}得知Lambda对应的接口：`new Thread(() -> System.out.println("Lambda表达式").start();`



代码：

```java
public class RunLambdaT {

    public static void main(String[] args) {

        //使用Lambda表达式调用me1
        me1(() -> {
            System.out.println("Lambda表达式输出");
        });

        //Lambda表达式继续简化
        me1(() -> System.out.println("Lambda表达式输出"));

        //如果调用me2, m2的参数接口中，有两个抽象方法，会报错
//        me2( ()-> System.out.println("不能使用Lambda表达式") );

        //必须有上下问环境，才能推导出Lambda对应的接口
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("启动多线线程01");
            }
        }).start();//启动多线程

        //()-> System.out.println("这样不行");
        Runnable r = () -> System.out.println("启动多线线程02");
        new Thread(r).start();

        //简化
        new Thread(() -> System.out.println("启动多线线程03")).start();


    }

    //只有一个抽象方法的接口 作为 参数传递
    private static void me1(MyInterfaceMe1 e) {
        e.method();
    }

    //有两个抽象方法的接口 作为 参数传递
    private static void me2(MyInterfaceMe2s e) {
        e.method1();
    }
}

//=========================================================================================

public interface MyInterfaceMe1 {
    void method();
}

//=========================================================================================

public interface MyInterfaceMe2s {
    void method1();
    void method2();
}

```





<hr>





## 方法引用🍥格式

1.2方法引用符
方法引用符
·:该符号为引用运算符，而它所在的表达式被称为方法矧引用

回顾一下我在体验方法引用中的代码：

- Lambda表达式：`usePrintable(s -> System.out.println(s));`
  - 分析：拿到参数s之后通过Lambda表达式，传递给`System.out.println`方法去处理

- 方法引用：usePrintable(System.out{% label ::  purple %}printIn);
  - 分析：直接使用System.out中的println方法来取代Lambda,代码更加的简洁  

推导与省略

- 如果使用Lambda,那么根据“可推导就是可省略”的原则，无需指定参数类型，也无需指定的重载形式，它们都将被自推导
- 如果使用方法引用，也是同样可以根据上下文进行推导
- 方法引用是Lambda的孪生兄弟...

<hr>

引用类方法，其实就是引用类的静态方法

- 格式：`类名::静态方法`
- 范例：`Integer::parselnt`
  - Integer类的方法：`public static int parselnt(String s)`将此String转换为int类型数据

练习：

- 定义一个接口(Converter),里面定义一个抽象方法
  - `int convert(String s);`
- 定义一个测试类(ConverterDemo),在测试类中提供两个方法
- 个方法是：`useConverter(Converter c)`
- 一个方法是主方法，在主方法中调用useConverter方法

<hr>


引用类的实例方法，其实就是引用类中的成员方法

- 格式：类名：成员方法
- 范例：`String::substring`
  - String类中的方法：public String substring(int beginlndex, int endlndex)`
  - 从beginIndex开始到endIndex结束，截取字符串。返回一个子串，子串的长度为endlndex-beginlndex

练习
- 定义一个接口(MyString),里面定义一个抽象方法：
  - `String mySubString(String s, int x, int y);`
- 定义一个测试类(MyStringDemo),在测试类中提供两个方法
  - 一个方法是：`useMyString(MyString my)`
  - 一个方法是主方法，在主方法中调用useMyString方法

```java
useMyString((s,x,y) -> s.substring(x,y));

//引用类的实例方法
useMyString(String:substring);
//Lambda表达式被类的实例方法替代的时候
//第一个参数作为调用者
//后面的参数全部传递给该方法作为参数
```



代码

```java
package 基础知识.Lambda表达式.A6_体验方法引用;


public class RunMethod {

    public String string = "阿西吧吧西巴西巴";

    public static void main(String[] args) {

        //使用Lambda表达式
        method1(e -> System.out.println(e));
        System.out.println("=================");

        //接着使用方法引用
        method1(System.out::println);//使用方法引用。
        System.out.println("=================");

        //通过类的静态方法的调用 实现方法引用
        method1(MethodInterface01::me);//自动识别参数类型 String
        System.out.println("=================");
        method1(RunMethod::me);//自动识别参数类型 String
        System.out.println("=================");

        //通过对象调用该对象方法 实现方法引用
        Me meObj = new Me();
        method1(meObj::method);
        System.out.println("=================");

        //String中用一个静态方法substring 参数列表有(String,int,int)
        subString((s,i,eI)->s.substring(i,eI));
        System.out.println("=================");
        subString(String::substring);
        //注意/解释:
        //Lambda表达式被类的实例方法替代的时候
        //第一个参数作为调用者
        //后面的参数全部按顺序传递给该方法作为参数。
        System.out.println("=================");



    }

    private static void subString(MySubString e){
        String str = e.meSubString("零一二三四五六七八", 3, 7);
        System.out.println(str);
    }

    private static void method1(MethodInterface01 e) {
        e.method("这就尴尬了");
    }


    private static int method2(MethodInterface02 e) {
        int i = e.method(6666);
        return i;
    }


    //该类中的两个重载方法
    public static void me(String str) {
        System.out.println("me重载 参数类型: String :" + str);
        System.out.println("RunMethod 内部默认方法。");
    }

    public static void me(int a) {
        System.out.println("me重载 参数类型: int :" + a);
        System.out.println("RunMethod 内部默认方法。");
    }

}
```

MethodInterface01

```java
package 基础知识.Lambda表达式.A6_体验方法引用;

public interface MethodInterface01 {
    void method(String s);//抽象方法
    static void me(String str){
        System.out.println("me重载 参数类型: String :" + str);
        System.out.println("MethodInterface01 内部默认方法。");
    }
    static void me(int a){
        System.out.println("me重载 参数类型: int :" + a);
        System.out.println("MethodInterface01 内部默认方法。");
    }
}
```

MethodInterface02

```java
package 基础知识.Lambda表达式.A6_体验方法引用;

public interface MethodInterface02 {
    int method(int a);
}
```

MySubString

```java
package 基础知识.Lambda表达式.A6_体验方法引用;

public interface MySubString {
    String meSubString(String str,int index,int endIndex);
}
```

Me

```java
package 基础知识.Lambda表达式.A6_体验方法引用;

public class Me {
    public void method(String s){
        System.out.println("Me类, 获参String:" + s);
    }
    public void method(int s){
        System.out.println("Me类, 获参int:" + s);
    }
}
```





<hr>





## 引用构造器

引用构造器，其实就是引用构造方法

- 格式：`类名::new`
- 范例：`Student::new`



练习

- 定义一个类(Student),里面有两个成员变量(name,age) 并提供无参构造方法和带参构造方法，以及成员变量对应的get和set方法
- 定义一个接口(StudentBuilder),里面淀义一个抽象方法
  - `Student build(String name, int age);`
- 定义一个测试类(StudentDemo),在测试类中提供两个方法
  -  一个方法是：`useStudentBuilder(StudentBuilder s);`
  - 一个方法是主方法，在主方法中调用useStudentBuilder方法



代码

```java
package 基础知识.Lambda表达式.A7_引用构造器;

public class RunStudent {

    public static void main(String[] args) {

        //通过Lambda表达式创建
        /*useStudent( (name,age)->{
            Student stu = new Student(name,age);
            return stu;
        } );*/
        useStudent((name,age)->new Student(name,age));

        System.out.println("====================");

        //通过Lambda表达式，调用构造器
        useStudent(Student::new);
        //Lambda表达式被构造器代替的时候, 它的形式参数全部传递给构造器的参数
        //返回Student 创建Student 将参数按顺序传给public Student(String name,int age){//...}

    }


    private static void useStudent(StudentInterface e) {
        Student stu = e.builder("尴尬酱", 20);
        System.out.println("姓名: " + stu.getName() + ", 年龄: " + stu.getAge());
    }

}
```


```java
package 基础知识.Lambda表达式.A7_引用构造器;

public interface StudentInterface {
    
    Student builder(String name,int age);
    
}
```



```java
package 基础知识.Lambda表达式.A7_引用构造器;

public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```





<hr>





## 函数式接口🍥概述

函数式接口概述
函数式接口：{% label 有且仅有一个抽象方法的接口 green %}
Java中的函数式编程体现就是Lambda表达式，所以函数式接口就是可以适用于Lambda使用的接口
只有确保接口中有且仅有一个抽象方法，Java中的Lambda才能顺利地进行推导

如何检测一个接口是不是函数式接口呢？

- `@Functionallnterface`
- 放在接口定义的上方：如果接口是函数式接口，编译通过：如果不是，编译失败

{% label 注意 orange%}

- 我们自己定义函数式接口的时候，@Functionallnterface是可选的，就算我不写这个注解，只要保证满足函数式接口定
  义的条件，也照样是函数式接口。但是，建议加上该注解



代码

```java
package 基础知识.Lambda表达式.A8_函数式接口;

public class RunFunctional {

    public static void main(String[] args) {
        String str = "正在听《封锁我一生--王杰》";

        //通过Lambda表达式创建 函数式接口对象
        MyInterface mi1 = () -> System.out.println(str);

        //调用方法
        mi1.method();

    }
}
```

```java
package 基础知识.Lambda表达式.A8_函数式接口;

@FunctionalInterface//函数式接口的注解！
public interface MyInterface {

    void method();

    //void method1(); //注解报错！

}
```





<hr>



## 函数式接口🍥使用

### 函数式接口作为方法的参数

需求

- 定义一个类(RunnableDemo),在类中提供两个方法
  - 一个方法是：`startThread(Runnable r)`方法参数Runnable是一个函数式接口
  - 一个方法是主方法，在主方法中调用startThread方法

如果方法的参数是一个函数式接口，我们可以使用Lambda表达式作为参数传递

- `startThread( () -> System.out.println(Thread.currentThread().getName() + "线f程启动了"));`



代码

```java
package 基础知识.Lambda表达式.A9_函数式接口作为方法的参数;

public class RunThread {

    public static void main(String[] args) {

        //使用匿名内部类的方式
        useThread(new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+": 线程启动");
            }
        });

        //使用Lambda表达式 作为参数
        useThread( ()-> System.out.println(Thread.currentThread().getName()+": 线程启动"));

        //这个之前就学过了, 只是换了种说法而已！！！

    }

    private static void useThread(Runnable r){
        new Thread(r).start();
    }

}
```



### 函数式接口作为方法的返回值

需求

- 定义一个类(ComparatorDemo),在类中提供两个方法
  - 一个方法是：Comparator<String>getComparator(0方法返▣值Comparator是一个函数式接口
  - 一个方法是主方法，在主方法中调用getComparator,方法

如果方法的返回值是一个函数式接口，我们可以使用Lambd表达式作为结果返回

- ```java
  private static Comparator<String> getComparator(){
  	return (s1,s2) -> s1.length() - s2.length();
  }
  ```



代码

```java
package 基础知识.Lambda表达式.A10_函数式接口作为方法的返回值;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

public class RunList {

    public static void main(String[] args) {
        //创建数组
        ArrayList<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbbb");
        list.add("d");
        list.add("cc");

        //排序前的集合
        System.out.println("排序前的集合:" + list);

        //自然排序后的集合
        Collections.sort(list);
        System.out.println("自然排序后的集合:" + list);

        //使用比较器排序后的集合
        Collections.sort(list, getComparator02());
        System.out.println("比较器排序后的集合:" + list);


    }


    //Comparator接口是函数式接口:  @FunctionalInterface

    //使用匿名内部类
    private static Comparator<String> getComparator01() {
        //匿名内部类作为返回值
        return new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o1.length() - o2.length();
            }
        };
    }

    //使用Lambda表达式作为参数返回值
    private static Comparator<String> getComparator02() {
        //compare(String o1,String o2)
        return (o1, o2) -> o1.length() - o2.length();
    }

}
```





<hr>



 

## 常用函数式接口


Java8在java.util.function包下预定义了大量的函数式接口供我们使用

重点来学习下面的4个接口

- Supplier接口
- Consumer接口
- Predicate接口
- Function:接口



### Supplier接口


`Supplier<T>`: 包含一个无参的方法

- Tget(): 获得结果
- 该方法不需要参数，它会按照某种实现逻辑（由Lambda表达式实现）返回一个数据
- `Supplier<T>`接口也被称为生产型接口，如果我们指定了接口的泛型是什么类型，那么接口中的get方法就会生产什么类型的数据供我们使用

练习

- 定义一个类(SupplierTest),在类中提供两个方法
  - 一个方法是：`int getMax(Supplier<Integer> sup)`用于返回一个int数组中的最大值
  - 一个方法是主方法，在主方法中调用getMax方法



代码

```java
package 基础知识.Lambda表达式.A12_常用函数式接口.Supplier接口;

import java.util.function.Supplier;

public class TestSupplier {

    public static void main(String[] args) {

        //使用Lambda表达式进行调用
//        getString( () -> {
//            return "Supplier接口";
//        } );

        //Lambda表达式简写
        String str = getString(() -> "Supplier函数式接口");
        System.out.println(str);

        //Lambda表达式获取
        Integer anInt = getInt(() -> 666);
        System.out.println(anInt);


    }

    //使用Supplier接口, 获取一个字符串
    private static String getString(Supplier<String> sup){
        return sup.get(); //Supplier里面有一个get()方法 , 用于返回泛型内容
    }

    //使用Supplier接口, 获取一个Integer
    private static Integer getInt(Supplier<Integer> sup){
        return sup.get(); //Supplier里面有一个get()方法 , 用于返回泛型内容
    }

}
```



### Consumer接口

`Consumer<T>`: 包含两个方法

- void accept(T t): 对给定的参数执行此操作
- default Consumer<T>andThen(Consumer after): 返回一个组合的Consumer, 依次执行此操作，然后执行after操作
- **Consumer<T>接口也被称为消费型接口，它消费的数据的数据类型由泛型指定**

练习

- Stringt[] strArray={"林青霞,30", "张曼玉,35", "王祖贤,33"};
- 字符串数组中有多条信息，请按照格式：“姓名：XX,年龄：XX"的格式将信息打印出来
- 要求：
  - 把打印姓名的动作作为第一个Consumer接口的Lambda实例
  - 把打印年龄的动作作为第二个Consumer接口的Lambda实例
  - 将两个Consumer接口按照顺序组合到一起使用



代码

```JAVA
package 基础知识.Lambda表达式.A12_常用函数式接口.Consumer接口;

import java.util.function.Consumer;

public class TestConsumer {

    public static void main(String[] args) {

        //使用Lambda表达式
        setAccept( "Saber",(s)-> System.out.println(s) );
        //使用Lambda 使用方法引用 简写。
        setAccept("GanGa",System.out::println);

        System.out.println("======================");

        //消费者 操作
        /*setAccept("尴尬酱",s -> {
            System.out.println(new StringBuilder(s).reverse().toString());
        });*/
        setAccept("尴尬酱",(s) -> System.out.println(new StringBuilder(s).reverse().toString()) );

    }

    private static void setAccept(String name, Consumer<String> consumer){
        consumer.accept(name);
    }

}
```



### Predicate


Predicate.<T>:常用的四个方法

- `boolean test(T t)`: 对给定的参数进行判断（判断逻辑由Lambda表达式实现见，返回一个布尔值
- `default Predicate<T> negate()`: 返回一个逻辑的否定，对应逻辑非
- `default Predicate<T> and(Predicate other)`: 返回一个组合判断，对应短路与
- `default Predicate<T> or(Predicate other)`: 返回一个组合判断，对应短路或
- **Predicate<T>接口通常用于判断参数是否满足指定的条件**

练习

- `String() strArray={"林青霞,30","柳岩,34"，"张曼玉,35"，"貂蝉,31"，"王祖贤,33");`
- 字符串数组中有多条信息，请通过Predicate接口的拼装将符合要求的字符串筛选到集合ArrayList中，并遍历ArrayList集合
- 同时满足如下要求：姓名长度大于2；年龄大于33
- 分析
  - 有两个判断条件，所以需要使用两个Predicate接口，对条件进行判断
  - 必须同时满足两个条件所以可以使用and方法连接两个判断条件



代码

```java
package 基础知识.Lambda表达式.A12_常用函数式接口.Predicate接口;

import java.util.function.Predicate;
/*
    boolean    test(T t) 在给定的参数上评估这个谓词。

    //逻辑判断 非
    default Predicate<T> negate() 返回表示此谓词的逻辑否定的谓词。

    //逻辑判断 与
    default Predicate<T> and(Predicate<? super T> other)
          返回一个组合的谓词，表示该谓词与另一个谓词的短路逻辑AND。

    //逻辑判断 或
    default Predicate<T> or(Predicate<? super T> other)
            返回一个组合的谓词，表示该谓词与另一个谓词的短路逻辑或。

*/

public class TestPredicate {

    public static void main(String[] args) {

        //使用Lambda表达式
        boolean p1 = pre1("这就尴尬了", (s) -> {
            return s.length() >= 6;
        });
        System.out.println(p1);//false

        //Lambda表达式 简写
        p1 = pre1("一点都不尴尬", s -> s.length() >= 6);
        System.out.println(p1);//true

        //=======================================================

        boolean p2 = pre2("一点都不尴尬", (s -> s.length() >= 6));
        System.out.println(p2);//false

        //=======================================================

        boolean p3 = pre3("一点都不尴尬", (s) -> s.length() < 4, s -> s.length() < 8);
        System.out.println(p3);//false

        //=======================================================

        boolean p4 = pre4("一点都不尴尬", (s) -> s.length() < 4, s -> s.length() < 8);
        System.out.println(p4);//true

    }

    //TestMe 在给定的参数上评估这个谓词。
    private static boolean pre1(String str, Predicate<String> e) {
        //boolean  test(T t) 在给定的参数上评估这个谓词。
        return e.test(str);
    }

    //negate 逻辑非
    private static boolean pre2(String str, Predicate<String> e) {
        //return !e.test(str);
        //default Predicate<T> negate() 返回表示此谓词的逻辑否定的谓词。
        return e.negate().test(str);
        //使用方法 只是非 仍要调用test 先调用negate negate再调用test方法
    }

    //and 逻辑与
    private static boolean pre3(String str, Predicate<String> e1, Predicate<String> e2) {
        //boolean b1 = e1.test(str);
        //boolean b2 = e2.test(str);
        //return b1 && b2;

        //default Predicate<T> and(Predicate<? super T> other)
        //返回一个组合的谓词，表示该谓词与另一个谓词的短路逻辑AND。
        return e1.and(e2).test(str);
    }

    //or 逻辑或
    private static boolean pre4(String str,Predicate<String> e1,Predicate<String> e2){
        //boolean b1 = e1.test(str);
        //boolean b2 = e2.test(str);
        //return b1 && b2;

        //default Predicate<T> and(Predicate<? super T> other)
        //返回一个组合的谓词，表示该谓词与另一个谓词的短路逻辑AND。
        return e1.or(e2).test(str);
    }

}
```



### Function接口

Function<T,R>: 常用的两个方法

- `R apply(T t)`:将此函数应用于给定的参数
- `default<V> Function andThen(Function after)`:返回一个组合函数，首先将该函数应用于输入，然后将after函数应用于结果
- **`Function<T,R>`接口通常用于对参数进行处理，转换处理逻粗由Lambda表达式实现)，然后返回一个新的值**



练习

- `String s = "林青霞30";`
- 请按照我指定的要求进行操作：
  1. 将字符串截取得到数字年龄部分
  2. 将上一步的年龄字符串转换成为int类型的数据
  3. 将上一步的int数据加70，得到一个int结课，在控制台输出
- 请通过Function接口来实现函数拼接



```java
package 基础知识.Lambda表达式.A12_常用函数式接口.Function接口;

import java.util.function.Function;

public class TestFunction {

    public static void main(String[] args) {

        //使用Lambda表达式
        convert("4399", (String s) -> {
            return Integer.parseInt(s);
        });
        //优化
        convert("3213", s -> Integer.parseInt(s));
        //再次优化 类方法引用
        convert("6666", Integer::parseInt);

        System.out.println("===================================");

        convert(4567, i -> String.valueOf(i + 8));

        System.out.println("===================================");

        converts("8838", Integer::parseInt, s -> String.valueOf(s + 10));

    }

    //定义一个方法,把一个字符串转为int类型, 在控制台输出
    private static void convert(String s, Function<String, Integer> function) {
        int i = function.apply(s);
        //这里只是调用apply转换接收
        //具体实现实在Lambda表达式当中来操作的。
        System.out.println(i);
    }

    //定义一个方法,把一个int类型的数据加上一个整数之后, 转为字符串在控制台输出
    private static void convert(int i, Function<Integer, String> function) {
        String str = function.apply(i);
        //这里只是调用apply转换接收
        //具体实现实在Lambda表达式当中来操作的。
        System.out.println(str);
    }

    //定义一个方法,把一个字符串转为int类型, int类型的数据加上一个整数之后再转为字符串, 在控制台输出
    private static void converts(String str, Function<String, Integer> e1, Function<Integer, String> e2) {
        int i = e1.apply(str);
        String newStr = e2.apply(i);
        //具体实现实在Lambda表达式当中来操作的。
        System.out.println("newString:"+newStr);
    }

}
```





## 单词复习



|          单词           |      翻译//解释      |
| :---------------------: | :------------------: |
|                         |                      |
|         Lambda          |        **λ**         |
| **FunctionalInterface** |  **函数式接口注解**  |
|     **Comparable**      |     **自然排序**     |
|     **Comparator**      |    **比较器排序**    |
|                         |                      |
|      **Supplier**       |   **供应商 接口**    |
|                         |                      |
|      **Consumer**       |   **消费者 接口**    |
|       **accept**        |       **接受**       |
|                         |                      |
|      **Predicate**      |  **谓语判断 接口**   |
|        **test**         | **测试 // 最终判断** |
|       **negate**        |   **取消 // 求非**   |
|      **and / or**       | **逻辑与 / 逻辑或**  |
|                         |                      |
|      **Function**       |  **类型转换 接口**   |
|        **apply**        |       **申请**       |
|                         |                      |





