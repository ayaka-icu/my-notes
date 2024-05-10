---
title: Java集合体系👻复习
date: 2021-05-20 00:00:00
tags: ["后端","Java基础","Java集合"]
cover: "
https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/集合类体系结构.png"
categories:
  - 后端
  - Java
  - Java基础

---





# 🌸集合体系总复习



## 整理导图

我整理的导图：

![集合类体系结构](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/集合类体系结构.png)





# 🌸Collection集合体系



## Collection接口



![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img-16351633468241.png)



代码

```java
package 基础知识.集合.Collection.Collection接口;

import java.util.ArrayList;
import java.util.Collection;

/* API中介绍:
Collection 层次结构 中的根接口。Collection 表示一组对象，这些对象也称为 collection 的元素。一些 collection 允许有重复的元素，而另一些则不允许。
一些 collection 是有序的，而另一些则是无序的。JDK 不提供此接口的任何直接 实现：它提供更具体的子接口（如 Set 和 List）实现。
此接口通常用来传递 collection，并在需要最大普遍性的地方操作这些 collection。
*/
/*
Collection不能直接实现，只能实现子接口[List]和[Set]两个子接口。
java.util.Collection           Collection<泛型>

Collection是单列集合的顶层接口，它表示一组对象，这些对象也称为Collection的元素。
JDK不提供此接口的任何直接实现，它提供更具体的子接口(List和Set)实现。

创建Collection集合的对象。
-使用多态的方法
-具体实现类List的ArrayList

=============================================================================================================
Collection集合常用方法

   boolean add(E e)            添加元素
   boolean remove(Object o)    从集合中移除指定的元素 【参数不是索引值，是对象元素内容】
   void clear()                清空集合中的元素
   boolean contains(Object o)  判断集合中是否存在指定的元素
   boolean isEmpty()           判断集合是否为空
   int size()                  集合的长度，也就是集合中元素的个数。

=============================================================================================================
使用快捷键ALT+7 打开结构，可以看到类的所有信息 方法

*/
public class Collection_Lei {

    public static void main(String[] args) {
        //使用多态的写法
        Collection<String> arrayList = new ArrayList<String>();
        arrayList.add("问君能有几多愁，");
        arrayList.add("恰似一江春水向东流。");
        System.out.println(arrayList);
        //对象是new出来的，直接打印的是地址值，如果不是，重写了toString()方法

        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>");

        //boolean add(E e) 添加元素
        boolean boo = arrayList.add("流呀流流到外婆桥");
        System.out.println(boo+" "+arrayList);
        System.out.println("==============================================");

        //boolean remove(Object o) 从集合中删除指定的元素
        boolean boo1 = arrayList.remove(1);//是对象，不是索引值
        boolean boo2 = arrayList.remove("流呀流流到外婆桥");
        System.out.println(boo1 + " " + boo2 + arrayList);
        System.out.println("==============================================");

        //void clear() 清空集合中的元素
        arrayList.clear();
        System.out.println(arrayList);
        System.out.println("==============================================");

        //boolean contains(Object o) 判断集合中是否存在指定的元素
        arrayList.add("问君能有几多愁，");
        arrayList.add("恰似一江春水向东流。");
        boolean boole = arrayList.contains("恰似一江春水向东流。");
        System.out.println(boole+" "+arrayList);
        System.out.println("==============================================");

        //boolean isEmpty() 判断集合是否为空
        boolean booE = arrayList.isEmpty();
        System.out.println(booE + " " + arrayList);
        System.out.println("==============================================");

        //int size() 集合的长度，也就是集合中元素的个数。
        int size = arrayList.size();
        System.out.println(size + " " + arrayList);
        System.out.println("==============================================");
    }

}
```


```java
package 基础知识.集合.Collection.Collection接口;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class Iterator_Method {

    public static void main(String[] args) {

        Collection<String> list = new ArrayList<>();
        list.add("问世间情为何物，");
        list.add("直教人生死相许。");

        Iterator<String> it = list.iterator();
        /**
        list.iterator;返回的是Ite对象，Itr是Iterator<E>的实现类。
        所以说这里是用了多态的方法创建的
        */

        /*
        public Iterator<E> iterator() {
            return new Itr();  //返回的是Itr对象
        }
                      ?
        //Itr是ArrayList的内部类
        private class Itr implements Iterator<E>{
                //...
        }

        */
        System.out.println(it.next());
        System.out.println(it.next());
        //System.out.println(it.next());
        // NoSuchElementException: 表明枚举中没有更多的元素。

        System.out.println("===================================");

        list.add("本是两情相愿");
        list.add("又其在朝朝暮暮");
        Iterator<String> ite = list.iterator();
        //1
        if (ite.hasNext()){
            System.out.println(1+": "+ite.next());
        }
        //2
        if (ite.hasNext()){
            System.out.println(2+": "+ite.next());
        }
        //3
        if (ite.hasNext()){
            System.out.println(3+": "+ite.next());
        }
        //4
        if (ite.hasNext()){
            System.out.println(4+": "+ite.next());
        }
        //5
        if (ite.hasNext()){
            System.out.println(5+": "+ite.next());
        }
        //6
        if (ite.hasNext()){
            System.out.println(6+ite.next());	}

        //进行循环遍历
        System.out.println("=====使用while循环=====");
        //创建迭代器
        Iterator<String> itr = list.iterator();
        int i = 1;
        while (itr.hasNext()){
            String str = itr.next();//一般会先取值，再使用。
            System.out.println((i++)+": "+str);
        }

    }
}
```



案例

```java
package 基础知识.集合.Collection.Collection接口.Collection案例;

/*
案例要求:
创建一个存储学生对象的集合，存储三个学生对象，使用程序实现在控制台遍历该集合。
*/

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class Run_Collection {

    public static void main(String[] args) {
        //创建三个对象，何某某
        Student stu1 = new Student("何雪冲",16);
        Student stu2 = new Student("何雪虫",18);
        Student stu3 = new Student("何学匆",20);
        //创建学生集合
        Collection<Student> list = new ArrayList<>();
        list.add(stu1);
        list.add(stu2);
        list.add(stu3);
        //创建迭代器
        Iterator<Student> itr = list.iterator();
        //进行遍历
        while (itr.hasNext()){
            Student stu = itr.next();
            System.out.println("我叫: " + stu.getName() + ", 今年" + stu.getAge() + "岁了。");
        }


    }

}
```

```java
package 基础知识.集合.Collection.Collection接口.Collection案例;

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





----





## List集合

2.1List集合概述和特点
List集合概述

- 有序集合（也称为序列），用户可以精确控制列表中每个元素的插入位置。用户可以通过整数索引访问元素，
  并搜索列表中的元素
- 与Set集合不同，列表通常允许重复的元素



List集合特点

- 有序：存储和取出的元素顺序一致
- 可重复：存储的元素可以重复

 

![img_Method](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img_Method.png)

代码

```java
package 基础知识.集合.Collection.List.List集合;
/*
java.util.List集合 继承了 Collection接口

List集合概述:
-- 有序集合(也成为序列)，用户可以精确控制列表中每个元素的插入位置。用户也可以通过整数索引访问元素，比搜索列表中的元素。
-- 与Set集合不同，列表通常允许重复的元素。

List集合特点:
[有序性]: 存储和取出的元素顺序一致。   有索引值。
[可重复]: 存储的元素可以重复。

========================================================================================================================
*/

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class List_interface {

    public static void main(String[] args) {

        //使用多态的方法创建List对象。
        List<String> list = new ArrayList<>();
        //向集合中添加元素
        list.add("天青日照树下溪");
        list.add("孩童赤脚溪水欢");
        list.add("惊蛙扑通跳水去");
        list.add("溅水飞入湛蓝天");
        list.add("恰是夏日微风起");
        list.add("===========");
        //添加重复元素
        list.add("天青日照树下溪");
        list.add("孩童赤脚溪水欢");
        list.add("惊蛙扑通跳水去");
        list.add("溅水飞入湛蓝天");
        list.add("恰是夏日微风起");
        list.add("===========");
        //也重写了toString()方法
        System.out.println(list);

        System.out.println("==============================================================================");
        System.out.println("有序集合，可以通过索引值调用相应位置元素");
        System.out.println(list.get(0));
        System.out.println(list.get(1));
        System.out.println("==============================================================================");

        //遍历集合
        //使用索引循环遍历
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
        //使用迭代器遍历
        System.out.println("通过iterator迭代器遍历");
        Iterator<String> itr = list.iterator();
        while (itr.hasNext()){
            System.out.println(itr.next());
        }
    }
}
```

Method代码

```java
package 基础知识.集合.Collection.List.List集合;

import java.util.ArrayList;
import java.util.List;

/*

List集合特有方法:

void add(int index, E element)   再此集合中的指定位置插入指定的元素。
E remove(int index)              删除指定索引处的元素，返回被删除的元素。
E set(int index, E element)      修改指定位置处的元素，返回被修改的元素。
E get(int index)                 返回指定索引处的元素。

[注意]:
remove set get 方法的参数 index 可能会引发索引越界异常，使用时不能越界。
*/
public class List_interface_Method {

    public static void main(String[] args) {

        //void add(int index, E element)   再此集合中的指定位置插入指定的元素。
        System.out.print("1: ");
        List<Double> list1 = new ArrayList<>();
        list1.add(0.0);
        list1.add(1.0);
        list1.add(2.0);
        list1.add(3.0);
        list1.add(2,1.5);
        System.out.println(list1);//[0.0, 1.0, 1.5, 2.0, 3.0]
        System.out.println("list1[2] = "+list1.get(2));//结果是：1.5
        //也就是说，再index参数索引位置添加元素,最终这个元素将在此index索引位置，
        //而原来这个index位置及后面的元素将会向后移动一个说索引值。
        //list1.add(7,6.66);  //.IndexOutOfBoundsException 索引越界异常！！！

        System.out.println("==========================================================================");

        //E remove(int index)              删除指定索引处的元素，返回被删除的元素。
        System.out.print("2: ");
        List<Integer> list2 = new ArrayList<>();
        list2.add(0);
        list2.add(1);
        list2.add(2);
        list2.add(3);
        int LIST_2 = list2.remove(0);
        System.out.println(list2);//结果是: [1, 2, 3]
        System.out.println("被删除的元素是: "+ LIST_2);//被删除的元素是: 0

        System.out.println("==========================================================================");

        //E set(int index, E element)      修改指定位置处的元素，返回被修改的元素。
        System.out.print("3: ");
        List<Integer> list3 = new ArrayList<>();
        list3.add(0);
        list3.add(1);
        list3.add(2);
        list3.add(3);
        int LIST_3 = list3.set(0, 666);
        System.out.println(list3);//[666, 1, 2, 3]
        System.out.println("被修改的原元素是: "+LIST_3);//被修改的原元素是: 0

        System.out.println("==========================================================================");

        //E get(int index)                 返回指定索引处的元素。
        System.out.print("4: ");
        List<Integer> list4 = new ArrayList<>();
        list4.add(0);
        list4.add(1);
        list4.add(2);
        list4.add(3);
        System.out.print("[");
        for (int i = 0; i < list4.size(); i++) {
            if (i != (list4.size()-1)){
                System.out.print(list4.get(i)+",");
            } else {
                System.out.print(list4.get(i));
            }
        }
        System.out.println("]");

        System.out.println("==========================================================================");

    }

}
```

----





## 增强for循环

增强for:简化数组和Collection集合的遍历

- 实现Iterable接口的类允许其对象成为增强型for语句的目标
- 它是JDK5之后出现的，其内部原理是一个lterator迭代器



增强for的格式

- 格式：

  ```java
  for(元素数据类型变量名：数组或者Collection集合){
  	//在此处使用变量即可，该变量就是元素
  }
  ```

- 范例：

  ```java
    int0[] ar r= {1,2,3,4,5};
    for(int i:arr){
    	System.out.printIn(i);
    }
  ```

  



演示

```java
package 基础知识.集合.Collection.List.List集合.增强for循环;

/*
增强for循环
增强for: 简化数组和Collection集合的遍历。

Collection继承了Iterable接口
在【Iterable 接口里面一个 iterator()抽象方法。】！！！
实现这个接口允许对象成为 "foreach" 语句的目标。

[概述]:
1. 实现了Iterable接口的类允许其对象成为增强型for语句的目标
2. 它是JDK5之后出现的，其内部原理是一个Iterator迭代器。

[格式]:
for(元素数据类型 变量名: 数组/Collection集合){
   //使用变量即可，该变量就是元素
}

[]
*/
public class Foreach {

    public static void main(String[] args) {

        int [] array = {0,1,3,4,5,6};
        for (int num : array) {
            System.out.println(num);
        }
        System.out.println("====");
        //在IDEA里面输入foreach可以快速使用
        for (int num :
                array) {
            System.out.println(num);
        }

    }

}
```

----







## Iterator迭代器



Listlterator



Listlterator:列表迭代器

- 通过List集合的llistlterator0方法得到，所以说它是List集合特有的迭代器
- 用于允许程序员沿任一方向遍历列表的列表迭代器，在迭代期间修改列表，并获取列表中迭代器的当前位置



Listlterator中的常用方法

- E next():返回迭代中的下一个元素
- boolean hasNext():如果迭代具有更多元素，则返回true
- E previous():返回列表中的上一个元素
- boolean hasPrevious():如果此列表迭代器在相反方向遍历列表时具有更多元素，则返回true
- void add(E e):将指定的元素插入列表



演示

```java
package 基础知识.集合.Collection.List.List集合.ListIterator迭代器;
/*
java.util.ListIterator<E>
ListIterator也叫: 列表迭代器

[ListIterator概述]:

--- ListIterator是List的listIterator()方法得到的，是List特有的迭代器。
--- ListIterator继承了Iterator接口。
--- 用于允许程序员沿任意方向遍历列表迭代器，在迭代期间修改列表，并获得列表中迭代器的当前位置。


[ListIterator常用方法]:

--- E next()                返回的迭代器中的下一个元素。
--- boolean hasNext()       如果迭代器具有更多元素，则返回true
--- E previous()            返回列表中的上一个元素。
--- boolean hasPrevious()   如果此列表迭代器在相反方向遍历列表具有更多元素，则返回ture

--- void add(E e)           将指定的元素插入列表                        【重点！】
                            【注意】:用的是ListIterator的对象进行add添加元素，而不是原集合list添加。

*/
import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;

public class ListIterator_Method {

    public static void main(String[] args) {

        List<String> list = new ArrayList<>();
        list.add("亚索");
        list.add("永恩");
        list.add("万豪");

        //多态创建ListIterator对象。
        ListIterator<String> listItr = list.listIterator();
        //正向遍历
        while (listItr.hasNext()){
            String str = listItr.next();
            System.out.println(str);
        }
        System.out.println("========");
        //反向遍历
        while (listItr.hasPrevious()){
            String str = listItr.previous();
            System.out.println(str);
        }
        System.out.println("========");

        System.out.println("===========================================================================================");

        //刚创建的的ListIterator对象的时候，不能直接使用Previous方法。
        //迭代器位置在0索引位置，上一个元素是没有的。
        ListIterator<String> listIte = list.listIterator();
        while (listIte.hasPrevious()){
            String str = listIte.previous();
            System.out.println(str);
        }

        System.out.println("输出结果: ");
//      System.out.println( listIte.previous() );//异常出错: NoSuchElementException
        System.out.println( listIte.next() );//初始化第一个元素是[亚索]，返回元素后，迭代器会向后退1(索引+1)
        System.out.println( listIte.previous() );//迭代器位于第二个元素，执行后向前进1(索引-1)，返回前进后的元素[亚索]
        //报错
        //亚索
        //亚索

        /**
        ListIterator<String> ier = list.listIterator();
        while (true){
            while (listItr.hasNext()){
                String str = listItr.next();
                System.out.print(str);
            }
            System.out.print("==");
            while (listItr.hasPrevious()){
                String str = listItr.previous();
                System.out.print(str);
         }  }
        */
        System.out.print("重点: ");
        //重点add()方法
        ListIterator<String> listIterator = list.listIterator();
        while (listIterator.hasNext()){
            String str = listIterator.next();
            if (str.equals("亚索")){
                listIterator.add("牛批,卢本伟没有开挂！"); //用的是ListIterator的对象进行add添加元素，而不是原集合list添加。
            }
        }
        System.out.println(list);
    }

}
```





## 源码分析 (List)

代码

```java
//                                                                  [class]
public interface List<E>{                         
    Iterator<E> iterator();
    //boolean add(E e); //这次不是通过list添加的，用不到
    /**
    新添加 listIterator方法
    */
    ListIterator<E> listIterator();
}

public abstract class AbstractList<E>{
    protected transient int modCount = 0;
    //......
}   //ArrayList<E>的父类

//                                                                  [class]
public class ArrayList<E> extends AbstractList<E> implements List<E>{

    
    /**用不到
    //get方法
    public E get(int index) {
        Objects.checkIndex(index, size);
        return elementData(index);
    }

    //add方法
    public boolean add(E e) {
        modCount++;
        add(e, elementData, size);
        return true;
    }

    //iterator方法
    public Iterator<E> iterator() {
        return new Itr();
    }

    //Itr内部类继承Iterator接口                                      [class]
    private class Itr implements Iterator<E> {
    }
    */
    private class Itr implements Iterator<E> {
    }
    
    /*
    新添加 listIterator方法，ArrayList实现
    */
    public ListIterator<E> listIterator() {
        return new ListItr(0);
    }
    //ListItr继承了Itr
    private class ListItr extends Itr implements ListIterator<E> {
        
        public void add(E e) {
            checkForComodification();

            try {
                int i = cursor;
                ArrayList.this.add(i, e);
                cursor = i + 1;
                lastRet = -1;
                expectedModCount = modCount; //[重点]
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }
    }
    
    final void checkForComodification() {
        if (modCount != expectedModCount)
            throw new ConcurrentModificationException();
    }
}
```





> 不同点分析：
>
> `ListItr`中的add方法:
>
> ```java
> private class ListItr extends Itr implements ListIterator<E> {
> public void add(E e) {
>     checkForComodification();
>     try {
>         int i = cursor;
>         ArrayList.this.add(i, e);
>         cursor = i + 1;
>         lastRet = -1;
>         expectedModCount = modCount; //[重点]
>     } catch (IndexOutOfBoundsException ex) {
>         throw new ConcurrentModificationException();
>     }
> }
> }
> ```
>
> 其中add方法里面多了一步: `expectedModCount = modCount;` 重新就导致两数相等
> 则`modCount != expectedModCount`为`false`就不会抛出异常。

----







## List并发修改异常


并发修改异常

- ConcurrentModificationException

产生原因

- 迭代器遍历的过程中，通过集合对象修改了集合中元素的长度，造成了迭代器获取元素中判断预期修改值和实际修改值不一致

解决方案

- 用for循环遍历，然后用集合对象做对应的操作即可



案例代码

```java
package 基础知识.集合.Collection.List.List集合.List并发修改异常;
/*
现在有一个需求：
           我有一个集合: List<String> list = new ArrayList<String>();
           里面有三个元素: list.add("亚索");list.add("永恩");list.add("万豪");
           遍历集集合，得到每一个元素，看有没有元素[亚索]，如果有，我就添加一个元素[牛批,卢本伟没有开挂！]，请写代码实现
*/

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class List_Yi {

    public static void main(String[] args) {

        List<String> list = new ArrayList<>();
        list.add("亚索");
        list.add("永恩");
        list.add("万豪");

        //这个是【重点=！！！！！【并发修改异常】
        Iterator<String> itr = list.iterator();
//        while (itr.hasNext()){
//            String str = itr.next();
//            if (str.equals("亚索")){
//                list.add("牛批,卢本伟没有开挂！");
//            }
//        }
        /**异常报错：
        Exception in thread "main" java.util.ConcurrentModificationException
        at java.base/java.util.ArrayList$Itr.checkForComodification(ArrayList.java:1013)
        at java.base/java.util.ArrayList$Itr.next(ArrayList.java:967)
        at MyJava.基础知识.集合.Collection.List.List集合.List并发修改异常.List_Yi.main(List_Yi.java:24)

        异常分析在：List并发修改异常的源码.md 文件当中。
        */


        //正确使用:
        for (int i = 0; i < list.size(); i++) {
            String str = list.get(i);
            if (str.equals("亚索")){
                list.add("牛批,卢本伟没有开挂！");
            }
        }
        System.out.println(list);
        //for循环，y y d s
    }

}
```

分析

并发修改异常：

````java
/**异常报错：报错异常类ConcurrentModificationException
Exception in thread "main" java.util.ConcurrentModificationException
at java.base/java.util.ArrayList$Itr.checkForComodification(ArrayList.java:1013)
at java.base/java.util.ArrayList$Itr.next(ArrayList.java:967)
at A1_MyJava.基础知识.集合.Collection.List.List集合.List并发修改异常.List_Yi.main(List_Yi.java:24)
*/
````

程序原码(简略):

````java
//                                                                  [class]
public interface List<E>{                         
    Iterator<E> iterator();
    boolean add(E e);
}

public abstract class AbstractList<E>{
    protected transient int modCount = 0;
    //......
}   //ArrayList<E>的父类

//                                                                  [class]
public class ArrayList<E> extends AbstractList<E> implements List<E>{

    //get方法
    public E get(int index) {
        Objects.checkIndex(index, size);
        return elementData(index);
    }

    //add方法
    public boolean add(E e) {
        modCount++;
        add(e, elementData, size);
        return true;
    }

    //iterator方法
    public Iterator<E> iterator() {
        return new Itr();
    }

    //Itr内部类继承Iterator接口                                      [class]
    private class Itr implements Iterator<E> {

        int expectedModCount = modCount;          //定义次数
        /**
        modCount是实际修改集合的次数
        expectedModCount是预期修改集合的次数
        */
        //实现hasNext方法
        public boolean hasNext() {
            return cursor != size;
        }

        //实现next方法
        public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }

        //判断，抛出异常
        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
    }

}
````

分析:

>24行出现的问题:
>`String str = itr.next();`
>向上是ArrayList的内部类Itr的next方法问题。

>next方法先调用了`checkForComodification();`方法:
>
>```java
>class ArrayList<E>{
>final void checkForComodification() {
>   if (modCount != expectedModCount)
>       throw new ConcurrentModificationException();
>}
>}
>```

>当`modCount != expectedModCount`时，
>就抛出了`ConcurrentModificationException`异常

原因:

>`modCount`是实际修改集合的次数，`expectedModCount`是预期修改集合的次数

>预期修改集合的次数在`内部类Itr中`:`int expectedModCount = modCount;`

>在Itr当中，是将`实际修改集合次`数赋值给了`预期修改集合次数`
>,所以说一开始是一样的，不会报错，但是当进行某些操作时，可能会发生变化

>**变化:**
>
>`modCount是实际修改集合的次数`的来源在`ArrayList<E>`的父类`AbstractList<E>`当中
>
>`protected transient int modCount = 0;`protected修饰子类`ArrayList<E>`继承父类
>
>所以一开始，`modCount`是实际修改集合的次数，`expectedModCount`是预期修改集合的次数
>两个值都是相等的0，每次`Itr.next()`都要先执行`checkForComodification();`
>
>```java
>class ArrayList<E>{
>public E next() {
>     checkForComodification();
>     int i = cursor;
>     if (i >= size)
>         throw new NoSuchElementException();
>     Object[] elementData = ArrayList.this.elementData;
>     if (i >= elementData.length)
>         throw new ConcurrentModificationException();
>     cursor = i + 1;
>     return (E) elementData[lastRet = i];
> }
>}
>```
>
>当进行:
>
>```
>if (str.equals("亚索")){
>   list.add("牛批,卢本伟没有开挂！");
>}
>```
>
>操作时，调用了add方法，跟进add方法:
>
>```java
>class ArrayList<E>{
>public boolean add(E e) {
>     modCount++;
>     add(e, elementData, size);
>     return true;
>}
>}
>```
>
>add方法当中，出现了`modCount++;`也就是，实际修改集合次数加1，
>而`expectedModCount`是预期修改集合的次数没有加1，
>
>导致`modCount != expectedModCount`执行抛出异常

解决方案

>使用 get方法 和 for循环 进行解决
>
>```java
>class ArrayList<E>{
>public E get(int index) {
>     Objects.checkIndex(index, size);
>     return elementData(index);
>}
>}
>```
>
>get方法没有像next方法一样，get没有`checkForComodification();`
>判断是否相等，更不会抛出`ConcurrentModificationException`异常

>for循环y y d s
>
>增强for循环 yyds

----

<br>



## List子集合

List集合子类特点

List集合常用子类：ArrayList,LinkedList

- ArrayList:底层数据结构是数组，查询快，增删慢
- LinkedList:底层数据结构是链表，查询慢，增删快



### 📌ArrayList

前面已经讲过了

----

 

### 📌LinkedList



![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img-16351649612457.png)

代码

```java
package 基础知识.集合.Collection.List.List子集合.LinkedList;

import java.util.Iterator;
import java.util.LinkedList;

/*
List有的方法LinkedList也有就不写了

[LinkedList特有的方法]:

public void addFirst(E e)  在该列表开头插入指定的元素
public void addLast(E e)   将指定的元素追加到此列表的末尾

public E getFirst()        返回此列表中的第一个元素
public E getLast()         返回此列表中的最后一个元素

public E removeFirst()     从此列表中删除并返回第一个元素
public E removeLast()      从此列表中删除并返回最后一个元素

[First]第一           [List] 最后
=============================================================================================================
List有三种方法遍历集合,
所以LinkedList也有三种方法遍历集合

*/
public class LinkedList_Impl {

    public static void main(String[] args) {

        //创建列表
        LinkedList<String> linkedList = new LinkedList<>();
        linkedList.add("亚索");
        linkedList.add("哥哥");
        linkedList.add("你好浪呀");
        linkedList.add(1,"呦哟呦");
        System.out.println(linkedList);
        System.out.println("========================");

        //public void addFirst(E e)  在该列表开头插入指定的元素
        //public void addLast(E e)   将指定的元素追加到此列表的末尾
        linkedList.addFirst("哇！");
        linkedList.addLast("不过我稀饭");
        System.out.println(linkedList);
        System.out.println("========================");

        //public E removeFirst()     从此列表中删除并返回第一个元素
        //public E removeLast()      从此列表中删除并返回最后一个元素
        String strFirst = linkedList.removeFirst(); //移除第一个元素
        String strLast = linkedList.removeLast();   //移除最后一个元素
        System.out.println("移除第一个元素:" + strFirst);
        System.out.println("移除最后一个元素" + strLast);
        System.out.println(linkedList);
        System.out.println("========================");

        //public E getFirst()        返回此列表中的第一个元素
        //public E getLast()         返回此列表中的最后一个元素
        String strGetFirst = linkedList.getFirst();
        String strGetLast = linkedList.getLast();
        System.out.println("获取第一个元素:" + strGetFirst);
        System.out.println("获取最后一个元素:" + strGetLast);
        System.out.println(linkedList);
        System.out.println("========================");

        //==================================================================================================
        //三种方式遍历LinkedList集合
        System.out.println("三种方式遍历LinkedList集合");

        //1. 使用迭代器
        Iterator<String> itr = linkedList.iterator();
        while (itr.hasNext()){
            String str = itr.next();
            System.out.println(str);
        }
        System.out.println("========================");


        //2. 使用for循环遍历
        for (int i = 0; i < linkedList.size(); i++) {
            System.out.println(linkedList.get(i));
        }
        System.out.println("========================");

        //3. 使用增强for循环
        for (String name:
             linkedList) {
            System.out.println(name);
        }
        System.out.println("========================");

    }

}
```



----



## Set集合 哈希值

----

概述:

>  哈希值:是JDK根据对象的`地址`或者`字符串`或者`数字`算出来的`int类型的数值`
>
> `Object`当中有一个`hashCode()`方法，可以调用某处的哈希值
>
> `public int Object()` 返回该对象的哈希码值。
>
> ----

[注意]:

> 数字是用不了hashCode()方法的。
>
> 在默认情况下, hashCode()方法没有被覆盖重写的情况下:
>
> 1. 不同对象的哈希值是不同的。
>
> 2. 对于字符串来说，stu1对象中的字符串[何雪聪]和str字符串的[何雪聪]哈希值是相同的。
>
> 3. 不同的字符串的哈希值也有可能是相同的！
>
> ----



代码

```java
package 基础知识.集合.Collection.Set.Set集合.哈希值;

/*
int i = Object.hashCode();

[注意]:

数字是用不了hashCode()方法的。

在默认情况下, hashCode()方法没有被覆盖重写的情况下:
1. 不同对象的哈希值是不同的。
2. 对于字符串来说，stu1对象中的字符串[何雪聪]和str字符串的[何雪聪]哈希值是相同的。
3. 不同的字符串的哈希值也有可能是相同的！

*/
public class Run_HashCode {

    public static void main(String[] args) {

        //创建学生对象。
        Student stu1 = new Student("何雪聪", 20);
        int iCode1 = stu1.hashCode();
        int iCode2 = stu1.hashCode();
        if (iCode1 == iCode2) {
            System.out.println("同一个对象多次调用哈希值相同。");
        }

        //创建第二个学生对象
        Student stu2 = new Student("何雪聪", 20);
        int jCode1 = stu2.hashCode();
        int jCode2 = stu2.hashCode();
        if (jCode1 == jCode2) {
            System.out.println("同一个对象多次调用哈希值相同。");
        }
        System.out.println("========================");


        //在默认情况下，不同对象的哈希值是不同的
        //非特殊情况下，重写hashCode()方法。
        if (iCode1 == jCode1) {
            System.out.println("重写了hashCode()方法");
        } else if (iCode1 != jCode1) {
            System.out.println("不同对象的哈希值是不同的");
        }
        System.out.println("========================");
        System.out.println("字符串的哈希值");
        System.out.println("亚索: " + "亚索".hashCode());

        System.out.println("========================");
        System.out.println("重地: "+"重地".hashCode());
        System.out.println("通话: "+"通话".hashCode());
        if ("重地".hashCode() == "通话".hashCode()){
            System.out.println("不同的字符串的哈希值可能相同！");
        }

        System.out.println("========================");
        //对象中的字符串和常量池中的字符串
        String str = "何雪聪";
        System.out.println("何雪聪: " + str.hashCode());
        System.out.println("stu1对象的name: " + stu1.getName().hashCode());
        if (stu1.getName().hashCode() == "何雪聪".hashCode()){
            System.out.println("stu对象中的字符串[何雪聪]和str字符串的[何雪聪]哈希值相同。");
        }

        int i = 1;
    }
}
```

----

```java
package 基础知识.集合.Collection.Set.Set集合.哈希值;

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
    /**
     * 重写hashCode方法
    @Override
    public int hashCode(){
        return 0;
    }
    */
}
```



Set集合概述和特点

Set集合特点

- 不包含重复元素的集合
- 没有带索引的方法，所以不能使用普通f和循环遍历



代码

```java
package 基础知识.集合.Collection.Set.Set集合;

/*
Set集合没什么太大的特点
当学完Collection接口时，就相当于学完Set集合
但要注意:
Set集合特点:
1. 不包含重复元素的集合
2. 没有带索引的方法, 所以不能使用普通for循环遍历。

HashSet: 它不保证 set 的迭代顺序

Set集合获取和传入都不带索引，而是和Collection一样(Object 0bj)为参数。

*/

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class Set_Interface {

    public static void main(String[] args) {

        //使用多态的方式创建集合对象
        Set<String> setSet = new HashSet<>();
        setSet.add("这就尴尬了");
        setSet.add("问君能有几多愁");
        setSet.add("恰似一江春水向东流");
        setSet.add("问君能有几多愁");//重复元素
        System.out.println(setSet);//[恰似一江春水向东流, 这就尴尬了, 问君能有几多愁]
                                   // 没有重复元素，也没有顺序
        System.out.println("================================================");
        System.out.println("================================================");

        //遍历集合
        //使用迭代器进行遍历
        Iterator<String> itr = setSet.iterator();
        while (itr.hasNext()){
            System.out.println(itr.next());
        }
        System.out.println("================================================");

        //使用增强for循环
        for (String name :
                setSet) {
            System.out.println(name);
        }
        System.out.println("================================================");

        System.out.println(setSet.hashCode());
    }

}
```



----





## Set子集合



### 📌HashSet

HashSet集合概述和特点

- 底层数据结构是哈希表
- 对集合的迭代顺序不作任何保证，也就是说不保证存储和取出的元素顺序致
- 没有带索引的方法，所以不能使用普通for循环遍历
- 由于是Set集合，所以是不包含重复元素的集合



代码

```java
package 基础知识.集合.Collection.Set.Set子集合.HashSet;
/*
[HashSet集合特点]:

1. 底层数据结构是哈希表
2. 对集合的迭代顺序不作任何保证, 也就是说不保证存储和取出的元素顺序一致
3. 没有带索引的方法, 所以不能使用普通for循环遍历
4. 由于是Set集合, 所以是不包含重复元素的集合

*/

import java.util.HashSet;
import java.util.Iterator;

public class HashSet_Impl {

    public static void main(String[] args) {

        //创建对象
        HashSet<String> hashSet = new HashSet<>();
        //添加元素
        hashSet.add("亚索");
        hashSet.add("哥哥");
        hashSet.add("你好浪");
        hashSet.add("亚索");//重复元素
        hashSet.add("哥哥");//重复元素
        System.out.println("================");
        System.out.println(hashSet.add("亚索"));//重复元素

        //遍历
        Iterator<String> itr = hashSet.iterator();
        while (itr.hasNext()){
            System.out.println(itr.next());
        }
        System.out.println("=======");
        for (String name :
                hashSet) {
            System.out.println(name);
        }


    }

}
```

源码分析

![源码分析](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/源码分析.png)

案例练习

![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img-16352175226764.png)

```java
package 基础知识.集合.Collection.Set.Set子集合.HashSet.案例练习;

import java.util.HashSet;
import java.util.Iterator;

public class HashSet_Stu {

    public static void main(String[] args) {

        HashSet<Student> hashSet = new HashSet<Student>();
        Student stu1 = new Student("尴尬",18);
        Student stu2 = new Student("尴尬酱",19);
        Student stu3 = new Student("尴尬帝",20);

        Student stu4 = new Student("尴尬",18);

        hashSet.add(stu1);
        hashSet.add(stu2);
        hashSet.add(stu3);

        hashSet.add(stu4); /**
        不同对象的地址值是不会重复的，所以stu1 和 stu3 不属于重复元素
        如果想达到要求：【对象的参数相同，即为同一个对象】 再Student类当中重写equals()和hashCode()方法
        */

        for (Student stu: hashSet){
            System.out.println("我叫："+stu.getName()+",年龄："+stu.getAge());
        }

        System.out.println("==============================================");

        Iterator<Student> ite = hashSet.iterator();
        while (ite.hasNext()){
            Student stu = ite.next();
            System.out.println("我叫："+stu.getName()+",年龄："+stu.getAge());
        }

    }

}
```

```java
package 基础知识.集合.Collection.Set.Set子集合.HashSet.案例练习;

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

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Student student = (Student) o;

        if (age != student.age) return false;
        return name != null ? name.equals(student.name) : student.name == null;
    }

    @Override
    public int hashCode() {
        int result = name != null ? name.hashCode() : 0;
        result = 31 * result + age;
        return result;
    }
}
```



----



### 📌LinkedHashSet

LinkedHashSet集合概述和特点

- 哈希表和链表实现的St接口，具有可预测的迭代次序
- 由链表保证元素有序，也就是说元素的存储和取出顺序是一致的
- 由哈希表保证元素唯一，也就是说没有重复的元素

 

代码

```java
package 基础知识.集合.Collection.Set.Set子集合.LinkedHashSet;

/*
LinkedHashSet集合概述和特点:
1. [哈希表]和[链表]实现的Set接口，具有可预测的迭代次序
2. 由[链表]保证元素有序，也就是说元素的存储和取出顺序是一致的
3. 由[哈希表]保证元素唯一，也就是说没有重复的元素。

*/

import java.util.LinkedHashSet;

public class LinkedHashSet_Lei {

    public static void main(String[] args) {

        //创建LinkedHashList集合对象
        LinkedHashSet<String> linkedHashSet = new LinkedHashSet<String>();

        //添加元素
        linkedHashSet.add("尴尬");
        linkedHashSet.add("问君能有几多愁");
        linkedHashSet.add("恰似一江春水向东流");
        linkedHashSet.add("尴尬");//重复元素

        //遍历集合
        for (String str :
                linkedHashSet) {
            System.out.println(str);
        }/*
        尴尬
        问君能有几多愁
        恰似一江春水向东流
        */
    }

}
```



----



### 📌TreeSet

TreeSet集合概述和特点

- 元素有序，这里的顺序不是指存储和取出的顺序，而是按照一定的规则进行排序，具体排序方式取决于构造方法
  - TreeSet():根据其元素的自然排序进行排序
  - TreeSet(Comparator comparator)：根据指定的比较器进行排序
- 没有带索引的方法，所以不能使用普通for循环遍历
- 由于是Set集合，所以不包含重复元素的集合



代码

```java
package 基础知识.集合.Collection.Set.Set子集合.TreeSet;

/*
TreeSet集合概述和特点:

1. 【元素是有序的】,这里的顺序不是指存储和取出的顺序，而是按照一定的规则进行排序，具体排序方式取决于构造方法。
     [无参构造]                         TreeSet(): 根据其元素的自然排序进行排序。
     [Comparator] TreeSet(Comparator comparator): 根据指定的比较器进行排序。

2. 没有带索引的方法，所以不能使用普通for循环遍历。

3. 由于是Set集合，所以不包含重复元素的集合。

*/

import java.util.TreeSet;

public class TreeSet_Lei {

    public static void main(String[] args) {

        //创建集合对象
        TreeSet<Integer> treeSet = new TreeSet<Integer>();

        //添加元素
        treeSet.add(30);
        treeSet.add(50);
        treeSet.add(10);
        treeSet.add(20);
        treeSet.add(40);

        treeSet.add(30);//重复元素
        treeSet.add(40);//重复元素

        //遍历集合
        for (int num :
                treeSet) {
            System.out.println(num);
        }/*
        10
        20
        30
        40
        50
        */

    }

}
```





----





### 📌TreeSet && 比较器类



自然排序 结论:

- 用TreeSet集合存储自定义对象，无参构造方法使用的是自然排序对元素进行排序的
- 自然排序，就是让元素所属的类实现Comparable接口，重写compareTo(To)方法
- 重写方法时，一定要注意排序规则必须按照要求的庄要条件和次要条件来写



代码

```java
package 基础知识.集合.Collection.Set.Set子集合.TreeSet.排序类.Comparable;
/*
[结论]:
1. 用TreeSet集合存储自定义对象，无参构造方法使用的是自然排序对元素进行排序的。
2. 自然排序，就是【让元素所属的类实现Comparable接口】，【重写compareTo(T o)方法】。
3. 重写方法时，一定要注意排序规则必须按照要求的【主要条件】和【次要条件】来写。
*/
import java.util.TreeSet;
public class TreeSet_Lei {

    public static void main(String[] args) {

        //添加try...catch异常处理
        try {
            Student_1 stu1 = new Student_1("GanGa", 18);
            Student_1 stu2 = new Student_1("GanGaJiang", 9);
            Student_1 stu3 = new Student_1("GanGaDi", 20);
            Student_1 stu4 = new Student_1("GanGaLe", 120);

            TreeSet<Student_1> tree = new TreeSet<>();
            tree.add(stu1);
            tree.add(stu2);
            tree.add(stu3);
            tree.add(stu4);

            for (Student_1 stu :
                    tree) {
                System.out.println(stu.getName() + "," + stu.getAge());
            }//ClassCastException 报错
        }catch(ClassCastException e){
            System.out.println("ClassCastException 报错");
        }

        //报错原因是：Comparable该接口对实现他的每一个类的对象强加一个整体排序
        //也就是说：  类<E>该类必须实现Comparable接口，并重写compareTo()方法

        System.out.println("===========================================================================================");

        //实现Comparable接口，并重写compareTo()方法
        Student_2 stu1 = new Student_2("GanGa", 18);
        Student_2 stu2 = new Student_2("GanGaJiang", 9);
        Student_2 stu3 = new Student_2("GanGaDi", 20);
        Student_2 stu4 = new Student_2("GanGaLe", 120);

        TreeSet<Student_2> tree = new TreeSet<>();
        tree.add(stu1);
        tree.add(stu2);
        tree.add(stu3);
        tree.add(stu4);

        for (Student_2 stu :
                tree) {
            System.out.println(stu.getName() + "," + stu.getAge());
        }
        /*
        重写compareTo()方法，返回值是
        return 0;  表示比较值结果相同，不添加该元素
        return 1;  表示比较值结果大于，添加该元素
        return -1; 表示比较值结果小于，添加该元素前面。

        三种方式运行结果分别为:
        return 0;
                    GanGa,18
        return 1;
                    GanGa,18
                    GanGaJiang,9
                    GanGaDi,20
                    GanGaLe,120
        return -1;
                    GanGaLe,120
                    GanGaDi,20
                    GanGaJiang,9
                    GanGa,18
        */

        System.out.println("==============================");


         /*
        [如果想按照年龄大小比较(升序排列)]:
            @Override
            public int compareTo(Student_2 o) {
                int num = this.age - o.age;
                return num;
            }
        this.age是此时添加的对象的age值，
        o.age是前元素的age值，
        当返回值大于零就放在后面，小于零就放在前面

        同理:[降序排列]
           @Override
            public int compareTo(Student_2 o) {
                int num = o.age - this.age;
                return num;
            }

         【注意】：只这样写，stu5是添加不了的，这里只是比较了年龄
         【解决方法】；附加条件，年龄相同时，比较名字排列
            @Override
            public int compareTo(Student_2 o) {
                int num = this.age - o.age;
                num1 = num == 0 ? this.name.compareTo(o.name) : num;
                return num1;
            }
        */
        Student_3 stu01 = new Student_3("GanGa", 18);
        Student_3 stu02 = new Student_3("GanGaJiang", 9);
        Student_3 stu03 = new Student_3("GanGaDi", 20);
        Student_3 stu04 = new Student_3("GanGaLe", 120);
        Student_3 stu05 = new Student_3("GanGaBaoBao",9);
        Student_3 stu06 = new Student_3("GanGaBaoBao",9);//重复

        TreeSet<Student_3> treeSet = new TreeSet<>();
        treeSet.add(stu01);
        treeSet.add(stu02);
        treeSet.add(stu03);
        treeSet.add(stu04);
        treeSet.add(stu05);
        treeSet.add(stu06);//重复

        for (Student_3 stu :
                treeSet) {
            System.out.println(stu.getName() + "," + stu.getAge());
        }/*
        GanGaBaoBao,9
        GanGaJiang,9
        GanGa,18
        GanGaDi,20
        GanGaLe,120
        */

        //回顾复习==============================================================================================
        System.out.println("==================================================================================");
        System.out.println("==================================================================================");

        TreeSet<Student_回顾复习> treeSet00 = new TreeSet<>();

        Student_回顾复习 student1 = new Student_回顾复习("GanGa", 18);
        Student_回顾复习 student2 = new Student_回顾复习("GanGaJiang", 9);
        Student_回顾复习 student3 = new Student_回顾复习("GanGaDi", 20);
        Student_回顾复习 student4 = new Student_回顾复习("GanGaLe", 120);
        Student_回顾复习 student5 = new Student_回顾复习("GanGaBaoBao",9);
        Student_回顾复习 student6 = new Student_回顾复习("GanGaBaoBao",9);//重复

        treeSet00.add(student1);
        treeSet00.add(student2);
        treeSet00.add(student3);
        treeSet00.add(student4);
        treeSet00.add(student5);
        treeSet00.add(student6);

        for (Student_回顾复习 stu :
                treeSet00) {
            System.out.println(stu.getName() + "," + stu.getAge());
        }

        System.out.println("=======================================");


    }

}
```

Student_1

```java
public class Student_1 {

    private String name;
    private int age;

    public Student_1(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public Student_1() {
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

Student_2

```java
package 基础知识.集合.Collection.Set.Set子集合.TreeSet.排序类.Comparable;

public class Student_2 implements Comparable<Student_2>{

    private String name;
    private int age;

    public Student_2(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Student_2() {
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

    @Override
    public int compareTo(Student_2 o) {
//        return 0;  //重复元素
//        return 1;  //放入后面
//        return -1; //放入前面
        return this.age - o.age;
    }

}
```

Student_3

```java
package 基础知识.集合.Collection.Set.Set子集合.TreeSet.排序类.Comparable;

public class Student_3 implements Comparable<Student_3>{

    private String name;
    private int age;

    public Student_3(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Student_3() {
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

    @Override
    public int compareTo(Student_3 o) {
        int num = this.age - o.age;
        int numL = num == 0 ? this.name.compareTo(o.name) : num;
        return numL;
    }

    /*@Override
    public int compareTo(Student_3 o) {
        int num = this.age - o.age;
        int num1 = num == 0 ? this.name.compareTo(o.name) : num;
        return num1;
    }*/

}
```

Student_回顾复习

```java
package 基础知识.集合.Collection.Set.Set子集合.TreeSet.排序类.Comparable;

public class Student_回顾复习 implements Comparable<Student_回顾复习> {

    private String name;
    private int age;

    public Student_回顾复习() {
    }

    public Student_回顾复习(String name, int age) {
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

    @Override
    public int compareTo(Student_回顾复习 o) {
        int num = this.age - o.age;
        int i = num == 0 ? this.name.compareTo(o.name) : num;
        return i;
    }
}
```



----



比较器排序



代码一

```java
package 基础知识.集合.Collection.Set.Set子集合.TreeSet.排序类.Comparator;

import java.util.Comparator;
import java.util.TreeSet;

public class TreeSet_Lei {

    public static void main(String[] args) {
                                                //构造方法的时候，创建一个比较排序接口的实现匿名类，泛型与TreeSet类型相同
        TreeSet<Student> treeSet = new TreeSet<>(new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                //这里变为调用前后两个对象的成员方法。因为成员变量私有化了
                int num = o1.getAge() - o1.getAge();
                int num1 = num == 0 ? o1.getName().compareTo(o2.getName()) : num;
                return num1;
            }
        });
        Student stu1 = new Student("GanGa", 18);
        Student stu2 = new Student("GanGaJiang", 9);
        Student stu3 = new Student("GanGaDi", 20);
        Student stu4 = new Student("GanGaLe", 120);
        Student stu5 = new Student("GanGaBaoBao", 9);
        Student stu6 = new Student("GanGaBaoBao", 9);//重复

        treeSet.add(stu1);
        treeSet.add(stu2);
        treeSet.add(stu3);
        treeSet.add(stu4);
        treeSet.add(stu5);
        treeSet.add(stu6);//重复

        for (Student stu :
                treeSet) {
            System.out.println(stu.getName() + "," + stu.getAge());
        }

    }

}
```

代码二

```java
package 基础知识.集合.Collection.Set.Set子集合.TreeSet.排序类.Comparator;

public class Student {

    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Student() {
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



----



案例复习 要求:

![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img-16352182727208.png)

代码一

```java
package 基础知识.集合.Collection.Set.Set子集合.TreeSet.排序类.案例;

import java.util.Comparator;
import java.util.TreeSet;

public class TreeSet_Lei {

    public static void main(String[] args) {

        //创建集合对象
        TreeSet<Student> treeSet = new TreeSet<Student>(new Comparator<Student>() {
            @Override //创建匿名内部类，并从写compare()方法，规定比较规则
            public int compare(Student s1, Student s2) {
                int num1 = (s2.getChinese() + s2.getMath()) - (s1.getChinese() + s1.getChinese()); //总分降序
                int num2 = num1 == 0 ? s2.getChinese() - s1.getChinese() : num1;                   //语文降序
                int num = num2 == 0 ? s1.getName().compareTo(s2.getName()) : num2;                 //姓名升序
                return num;
            }
        });

        //创建学生对象
        Student stu1 = new Student("GanGa", 100, 120);
        Student stu2 = new Student("GanGaJiang", 150, 150);
        Student stu3 = new Student("GanGaDi", 120, 120);
        Student stu4 = new Student("GanGaBaoBao", 1, 0);
        Student stu5 = new Student("GaGaLe", 150, 150);
        Student stu6 = new Student("GanGaWWW", 120, 100);
        Student stu7 = new Student("GanGaBaoB", 0, 1);
        Student stu8 = new Student("GanGaLou",100,120);


        //添加元素stu1
        treeSet.add(stu1);
        treeSet.add(stu2);
        treeSet.add(stu3);
        treeSet.add(stu4);
        treeSet.add(stu5);
        treeSet.add(stu6);
        treeSet.add(stu7);
        treeSet.add(stu8);

        //遍历集合
        for (Student stu : treeSet) {
            System.out.println(stu.getName());
            System.out.println("语文:" + stu.getChinese() + ",数学:" + stu.getMath() + "总分为：" + (stu.getChinese() + stu.getMath()));
            System.out.println();
        }

        //运行结果：
        //GaGaLe总分为：300
        //GanGaJiang总分为：300
        //GanGaDi总分为：240
        //GanGa总分为：220
        //GanGaBaoBao总分为：1

    }

}
```

代码二

```java
package 基础知识.集合.Collection.Set.Set子集合.TreeSet.排序类.案例;

public class Student {

    private String name;
    private int chinese;
    private int math;

    public Student() {
    }

    public Student(String name, int chinese, int math) {
        this.name = name;
        this.chinese = chinese;
        this.math = math;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getChinese() {
        return chinese;
    }

    public void setChinese(int chinese) {
        this.chinese = chinese;
    }

    public int getMath() {
        return math;
    }

    public void setMath(int math) {
        this.math = math;
    }
}
```







## Collection工具类



Collections概述

- 是针对集合操作的工具类

Collections类的常用方法

- `public static<T extends Comparable<？super T>>void sort(ist<T> list)`:将指定的列表按升序排序
- `public static void reverse(List<?>list)`:反转指定列表中元素的顺序
- `public static void shuffle(List<?> list)`:使用默认的随机源随机排列指定的列表



代码

```java
package 基础知识.集合.Collections工具类;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

/*
1:构造方法私有化，不能创建对象
private Collections() {
}

2:成员变量静态化; 成员方法静态化;

所以，Collections是一个工具类。

API描述：
此类完全由在 collection 上进行操作或返回 collection 的静态方法组成。
它包含在 collection 上操作的多态算法，即“包装器”，
包装器返回由指定 collection 支持的新 collection，以及少数其他内容。

说明:【Collections类是针对集合操作的工具类】

常用的方法
--public static <T extends Comparable<? sup T> > void sort(List<T> list): 将指定的列表按升序排列

--public static void reverse(List<?> list):            反转指定的列表中的元素的顺序。

--public static void shuffle(List<?> list):            使用默认的随机源随机排列指定的列表。
*/
public class TestCollection {
    //启动
    public static void main(String[] args) {
        new TestCollection().init();
    }

    public void init(){
        //初始化
        System.out.println("===============================================================");


        //先创建一个List集合
        List<String> list = new ArrayList<>();
        list.add("3问君能有几多愁");
        list.add("4剑圣塔下达不溜");
        list.add("1风萧萧兮易水寒");
        list.add("2壮士一去兮不复还");

        //通过Collections工具类修改数组

        //public static <T extends Comparable<? sup T> > void sort(List<T> list): 将指定的列表按升序排列
        Collections.sort(list);
        System.out.println(list);
        //[1风萧萧兮易水寒, 2壮士一去兮不复还, 3问君能有几多愁, 4剑圣塔下达不溜]

        System.out.println("===============================================================");

        //public static void reverse(List<?> list):            反转指定的列表中的元素的顺序。
        Collections.reverse(list);
        System.out.println(list);
        //[4剑圣塔下达不溜, 3问君能有几多愁, 2壮士一去兮不复还, 1风萧萧兮易水寒]

        System.out.println("===============================================================");

        //public static void shuffle(List<?> list):            使用默认的随机源随机排列指定的列表。
        Collections.shuffle(list);
        System.out.println(list);
        //[2壮士一去兮不复还, 1风萧萧兮易水寒, 3问君能有几多愁, 4剑圣塔下达不溜]

        System.out.println("===============================================================");

        List<String> listE;
        Collections.shuffle(list);
        System.out.println(list);
    }

}
```





## CollectionToComparator



![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img-163521860727110.png)

代码

```java
package 基础知识.集合.Collections工具类.CollectionToComparator;


import java.util.*;

public class TestCollectionToComparator {

    //启动类
    public static void main(String[] args) {
        new TestCollectionToComparator().init();
    }

    //初始化
    public void init(){

        //创建学生对象
        Student stu1 = new Student("尴尬酱", 18);
        Student stu2 = new Student("尴尬帝", 20);
        Student stu3 = new Student("尴尬了", 11);
        Student stu4 = new Student("这就尴", 20);
        Student stu5 = new Student("尴某某", 21);

        //创建ArrayList数组
        ArrayList<Student> list = new ArrayList<>();
        list.add(stu1);
        list.add(stu2);
        list.add(stu3);
        list.add(stu4);
        list.add(stu5);

        //进行排序
        Collections.sort(list, new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                int i = o1.getAge() - o2.getAge();
                int num = i == 0 ? o1.getName().compareTo(o2.getName()) : i;
                return num;
            }
        });
        Iterator<Student> itr = list.iterator();
        while (itr.hasNext()){
            Student stu = itr.next();
            System.out.println("姓名: "+stu.getName()+", 年龄: "+stu.getAge());
        }

    }
}
```

```java
package 基础知识.集合.Collections工具类.CollectionToComparator;

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





# 🌸Map集合体系

 

## Map集合概述与使用

Map集合概述

- `Interface Map<K,V>` K:键的类型；V:值的类型
- 将键映射到值的对象；不能包含重复的键；每个键可以映射到最多一个值
- 举例：学生的学号和姓名
  - itheima001  林青霞
  - itheima002  张曼玉
  - itheima003  王祖贤

创建Map集合的对象

- 多态的方式
- 具体的实现类HashMap



代码

```java
package 基础知识.集合.Map;
/*
Map集合概述:

1. interface Map<K,V> K: 键的类型;  V: 值的类型
2. 将键映射到值的对象; 不能包含重复的键; 每个键可以映射到最多一个值;

*/
import java.util.HashMap;
import java.util.Map;

public class MapLei {

    public static void main(String[] args) {

        //用多态的方法创建Map集合对象
        Map<String, String> map = new HashMap<String, String>();

        //添加映射 使用put方法： V put(K key, V value);
        map.put("吟留的诗人","温迪");
        map.put("天动万象","钟离");
        map.put("无想的一刀","巴尔");
        map.put("天动万象","摩拉克斯");//键值是唯一的，当有这个键时，后面的值会替换以前的值

        //输出打印
        System.out.println(map);
        //重写toString后的输出结果 ：{无想的一刀=巴尔, 吟留的诗人=温迪, 天动万象=钟离}
        //结果也是无序的。


    }

}
```



## Map集合方法



 ![imgMethod](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/imgMethod.png)

代码

```java
package 基础知识.集合.Map;

import java.util.HashMap;
import java.util.Map;

/*
Map集合的基本功能

V put(K key, V value)                 添加键值对
V remove(Object key)                  根据键删除键值对元素
void clear()                          移除所有的键值对元素
boolean containsKey(Object key)       判断集合是否包含指定的键
boolean containsKey(Object value)     判断集合是否包含指定的值
int size()                            集合的长度，也就是集合中键值对的个数
boolean isEmpty()                     判断集合是否为空
*/
public class MapMethod {

    public static void main(String[] args) {

        //多态创建Map集合对象
        Map<String, String> map = new HashMap<>();

        //V put(K key, V value)                 添加键值对
        map.put("吟留的诗人","温迪");
        map.put("天动万象","钟离");
        map.put("无想的一刀","巴尔");
        map.put("亡","草神");
        System.out.println(map);

        //V remove(Object key)                  根据键删除键值对元素
        map.remove("亡");
        System.out.println(map.remove("亡"));//已删除null

        //void clear()                          移除所有的键值对元素
        map.clear();
        System.out.println(map);

        //重新添加回来
        map.put("吟留的诗人","温迪");
        map.put("天动万象","钟离");
        map.put("无想的一刀","巴尔");
        System.out.println("重新添加回来：" + map);

        //boolean containsKey(Object key)       判断集合是否包含指定的键
        System.out.println(map.containsKey("无想的一刀"));
        System.out.println(map.containsKey("亡"));

        //boolean containsKey(Object value)     判断集合是否包含指定的值
        System.out.println(map.containsValue("巴尔"));
        System.out.println(map.containsValue("草神"));

        //int size()                            集合的长度，也就是集合中键值对的个数
        System.out.println("键值队个数：" + map.size());

        //boolean isEmpty()                     判断集合是否为空
        System.out.println(map.isEmpty());
        map.clear();
        System.out.println("clear后：" + map.isEmpty());


    }

}
```





----



## Map集合获取方法



![MapGetMethod](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/MapGetMethod.png)

代码

```java
package 基础知识.集合.Map;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

/*
Map集合获取功能:

V get(Object key)                  根据键获取值

Set<K> keySet()                    获取所有键的集合

Collection<V> values()             获取所有值的集合

Set<Map.Entry<K,V>> entrySet()     获取所有键值对对象的集合

*/
public class MapGetMethod {

    public static void main(String[] args) {

        //创建Map集合
        Map<String, String> map = new HashMap<>();

        //添加键值对
        map.put("吟留的诗人","温迪");
        map.put("天动万象","钟离");
        map.put("无想的一刀","巴尔");
        map.put("亡","草神");
        System.out.println("==================================");

        // V get(Object key)                  根据键获取值
        System.out.println(map.get("无想的一刀")); //巴尔
        System.out.println(map.get("天动万象"));  //钟离
        System.out.println(map.get("神罗天征"));  //null
        System.out.println("==================================");

        //Set<K> keySet()                    获取所有键的集合
        System.out.println(map.keySet());//可直接打印
        Set<String> setMap = map.keySet();//可创建集合获取
        System.out.println("键集合遍历:");
        for (String str :
                setMap) {
            System.out.println(str);
        }
        System.out.println("==================================");

        //Collection<V> values()             获取所有值的集合
        Collection<String> values = map.values();
        System.out.println("值集合便利:");
        for (String str :
                values) {
            System.out.println(str);
        }
        System.out.println("==================================");

    }

}
```



----



## Map集合遍历

### 方式一

```java
package 基础知识.集合.Map.Map遍历;

import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class Map01 {

    public static void main(String[] args) {

        //多态创建集合
        Map<String, Integer> map = new HashMap<>();

        //添加键值对
        map.put("尴尬酱", 1);
        map.put("尴尬了", 2);
        map.put("尴尬帝", 3);
        map.put("真尴尬", 4);
        map.put("贼尴尬", 5);

        //创建键的集合
        Set<String> key = map.keySet();

        //遍历
        for (String strKey: key){
            //通过获取的键，获得相应的值
            Integer value = map.get(strKey);
            //输出/遍历
            System.out.println(strKey+"="+value);
        }
    }
}
```

----



### 方式二

```java
package 基础知识.集合.Map.Map遍历;

import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class Map02 {

    public static void main(String[] args) {

        //多态创建集合
        Map<String, Integer> map = new HashMap<>();

        //添加键值对
        map.put("尴尬酱", 1);
        map.put("尴尬了", 2);
        map.put("尴尬帝", 3);
        map.put("真尴尬", 4);
        map.put("贼尴尬", 5);

        //创建键值对对象集合
        Set<Map.Entry<String, Integer>> entry = map.entrySet();

        //遍历
        for (/*Map.Entry<键,值>*/
            Map.Entry<String, Integer> me : entry) {
            //获得键和值
            String key = me.getKey();
            Integer value = me.getValue();
            System.out.println(key + "=" + value);
        }

    }

}
```

----





## Map集合练习



### 练习一

要求

![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img-16352402404542.png)

代码

```java
package 基础知识.集合.Map.Map练习.Map练习1;

import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class DameMap {

    public static void main(String[] args) {

        //创建集合
        HashMap<String, Student> map = new HashMap<>();

        //创建学生对象
        Student stu1 = new Student("尴尬酱", 18);
        Student stu2 = new Student("尴尬帝", 20);
        Student stu3 = new Student("尴尬了", 11);

        //添加到集合中
        map.put("20202218", stu1);
        map.put("20202220", stu2);
        map.put("20202211", stu3);

        //遍历方法 一
        Set<String> keySet = map.keySet();
        for (String id : keySet) {
            Student s1 = map.get(id);
            System.out.println("学号:" + id + ", 姓名:" + s1.getName() + ", 年龄:" + s1.getAge());
        }

        System.out.println("====================================================================");

        //遍历方法 二
        Set<Map.Entry<String,Student>> entry =  map.entrySet();
        for (Map.Entry<String,Student> em: entry){
            String key = em.getKey();
            Student value = em.getValue();
            System.out.println("学号:" + key + ", 姓名:" + value.getName() + ", 年龄:" + value.getAge());
        }

    }

}
```

```java
package 基础知识.集合.Map.Map练习.Map练习1;

public class Student {

    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    //GetSet方法对
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
````

----





### 练习二

要求

![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img-16352403589763.png)

代码

```java
package 基础知识.集合.Map.Map练习.Map练习2;

import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class DameHashMap {

    public static void main(String[] args) {

        new DameHashMap().init();

    }

    //初始化
    public void init() {

        //初始化
        System.out.println("===============================================");

        //创建HashMap集合
        HashMap<Student, String> hashMap = new HashMap<>();

        //创建学生对象
        //添加对象要保证成员变量和成员方法不同，保证唯一性
        //需要给Student_hash重写hashCode()和equals()方法。
        Student stu1 = new Student("尴尬酱", 18);
        Student stu2 = new Student("尴尬帝", 20);
        Student stu3 = new Student("尴尬了", 11);
        Student stu4 = new Student("尴尬了", 11);//重复元素对象

        //向hashMap集合当中添加键值对
        hashMap.put(stu1, "20202218");
        hashMap.put(stu2, "20202220");
        hashMap.put(stu3, "20202211");
        hashMap.put(stu4, "20202211");

        //直接打印hashMap
        System.out.println(hashMap);
        //结果：
        // {A1_MyJava.基础知识.集合.Map.Map练习.Map练习2.Student_hash@2b4d4e7e=20202211,
        //  A1_MyJava.基础知识.集合.Map.Map练习.Map练习2.Student_hash@2b5568fa=20202218,
        //  A1_MyJava.基础知识.集合.Map.Map练习.Map练习2.Student_hash@2b4f31d0=20202220}

        //重写了hashCode()和equals()方法后，对于属性（成员变量）相同时，视为相同元素

        //循环遍历 方法一
        Set<Student> keyId = hashMap.keySet();
        for (Student stu : keyId) {
            //得到对应的value值
            String id = hashMap.get(stu);
            //打印信息
            System.out.println("姓名为: " + stu.getName() + ", 年龄: " + stu.getAge() + ", 学号: " + id);
        }

        System.out.println("===============================================");

        //循环遍历 方法二
        Set<Map.Entry<Student, String>> hashEm = hashMap.entrySet();
        for (Map.Entry<Student, String> stuId : hashEm) {
            //获得键值
            Student key = stuId.getKey();
            String value = stuId.getValue();
            //打印信息
            System.out.println("姓名为: " + key.getName() + ", 年龄: " + key.getAge() + ", 学号: " + value);
        }


    }

}
```

```java
package 基础知识.集合.Map.Map练习.Map练习2;

import java.util.Objects;

public class Student {

    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    //GetSet方法对
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


    //重写equals()和hashCode()方法。
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student that = (Student) o;
        return age == that.age && Objects.equals(name, that.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```



----





### 练习三

要求

![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img-16352403697294.png)

代码

```java
package 基础知识.集合.Map.Map练习.Map练习3;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class DameHashMap {

    public static void main(String[] args) {
        //启动
        new DameHashMap().init();
    }

    //初始化
    public void init() {

        //初始化
        System.out.println("=========================");

        //创建ArrayList集合, 泛型为:< HapMap<T,V> >值。
        ArrayList<HashMap<String, String>> arrayMap = new ArrayList<>();

        //创建HashMap集合,并添加相应的键值对
        HashMap<String, String> map1 = new HashMap<>();
        map1.put("孙策", "大桥");
        map1.put("周瑜", "小桥");
        HashMap<String, String> map2 = new HashMap<>();
        map1.put("郭靖", "黄蓉");
        map1.put("杨过", "小龙女");
        HashMap<String, String> map3 = new HashMap<>();
        map1.put("亚索", "瑞文");
        map1.put("艾希", "蛮王");

        //向ArrayList中添加HashMap对象。
        arrayMap.add(map1);
        arrayMap.add(map2);
        arrayMap.add(map3);

        //遍历ArrayList    方法一
        for (HashMap<String, String> mapH1 : arrayMap) {
            //获取map中的键集合
            Set<String> keySet = mapH1.keySet();
            for (String keyL : keySet) {
                //获得键对应的值
                String valueL = mapH1.get(keyL);
                //打印结果
                System.out.println(keyL + " 喜欢 " + valueL);
                System.out.println("-------------");
            }
        }

        System.out.println("=========================");

        //遍历ArrayList    方法二
        for (HashMap<String,String> mapH2: arrayMap) {
            //获取当前的键值对集合
            Set<Map.Entry<String, String>> em = mapH2.entrySet();
            //遍历HashMap集合
            for (Map.Entry<String, String> keyToValue :em) {
                //获得键
                String key = keyToValue.getKey();
                String value = keyToValue.getValue();
                //打印结果
                System.out.println(key + " 喜欢 " + value);
                System.out.println("-------------");

            }
        }
    }


}
```



----



### 练习四

要求

![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img-16352403799095.png)

代码

```java
package 基础知识.集合.Map.Map练习.Map练习4;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class DameHashMap {

    //启动
    public static void main(String[] args) {
        new DameHashMap().init();
    }

    //初始化
    public void init(){
        //初始化
        System.out.println("==============================");

        //创建HashMap, 泛型为 < String, ArrayList<String> >
        HashMap<String, ArrayList<String>> map = new HashMap<>();

        //创建三个ArrayList并添加元素, 泛型为 < String >
        ArrayList<String> list1 = new ArrayList<>();
        list1.add("郭靖");
        list1.add("黄蓉");
        ArrayList<String> list2 = new ArrayList<>();
        list2.add("亚索");
        list2.add("瑞文");
        ArrayList<String> list3 = new ArrayList<>();
        list3.add("岩神");
        list3.add("雷神");

        //向HashMap集合中添加键，值为ArrayList
        map.put("相爱",list1);
        map.put("想杀",list2);
        map.put("故人",list3);

        //遍历HashMap集合     方法一
        Set<String> keySet = map.keySet();
        for (String keyL :keySet) {
            //获得键值
            ArrayList<String> valueL = map.get(keyL);
            //遍历集合
            for (String str :valueL) {
                //打印结果
                System.out.println("==="+keyL+"===");
                System.out.println(str);
            }
            System.out.println("===============");
        }

        System.out.println("==============================");

        //遍历HashMap集合     方法二
        Set<Map.Entry<String, ArrayList<String>>> em = map.entrySet();
        for (Map.Entry<String, ArrayList<String>> keyToValue :em) {
            //获得键
            String key = keyToValue.getKey();
            ArrayList<String> value = keyToValue.getValue();
            //遍历ArrayList
            for (String str :value) {
                //打印结果
                System.out.println("==="+key+"===");
                System.out.println(str);
            }
            System.out.println("===============");
        }

    }

}
```





----



### 练习五

要求

![img](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/img-16352403880276.png)

代码

```java
package 基础知识.集合.Map.Map练习.Map练习5;

import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;
import java.util.Set;

public class DameHashMap {

    public static void main(String[] args) {
        new DameHashMap().init();
    }

    public void init(){

        //创建字符串
        System.out.println("请输入:");
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();//扫描下一行，返回类型为String

        //创建一个HashMap集合
        HashMap<Character, Integer> map = new HashMap<>();

        //获得字符
        for (int i = 0; i<str.length(); i++){
            char key = str.charAt(i);

            Integer value = map.get(key);
            if (value == null){
                map.put(key,1);
            }else{
                map.put(key,map.get(key)+1);
            }

        }

        //遍历HashMap   方法一
        Set<Character> keySet = map.keySet();
        for (Character keyL :keySet) {
            //获得值
            Integer value = map.get(keyL);
            //打印结果
            System.out.print(new StringBuilder().append(keyL).append("(").append(value).append(")"));
            //这里使用了StringBuilder()类，方法是append();
        }
        System.out.println();
        System.out.println("=====================================================");

        //遍历HashMap   方法二
        Set<Map.Entry<Character, Integer>> em = map.entrySet();
        for (Map.Entry<Character, Integer> keyToValue :em) {
            //获取键和值
            Character key = keyToValue.getKey();
            Integer value = keyToValue.getValue();
            //遍历字符串
            System.out.print(new StringBuilder().append(key).append("(").append(value).append(")"));

        }
    }

}
```





----





# 🌸常见的数据类型

----



## 数据结构之【栈】

----

> ![栈](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/栈.png)
> **在内存进出两个过程:** <br>
>
> **[先进后出的模型]**<br>
>
> **数据进入栈模型的过程为: `【压/进 栈】`**             <br>
> **数据离开栈模型的过程为: `【弹/出 栈】`**
>
> ------
>
> **在进栈的时候，数据先进入栈的底部，`栈底元素——>栈顶元素`  **  <br>
> **在出栈的时候，数据先从栈最上部出，`栈顶元素——>栈底元素`**

-----





## 数据结构之【队列】

----

> ![数列](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/数列.png)
> **队列与栈恰好相反: ** 
>
> <br>**[先进先出的模型]** <br>
>
> **数据从后端进入队列模型的过程称为: `【入队列】`**       <br>
> **数据从后端离开队列模型的过程称为: `【出队列】`**
>
> ------
>
> **进队列与栈相同 ** <br>
> **出队列的时候是，`前端`(下部)先出**

-----







## 数据结构之【数组】

----

> ![数组](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/数组.png)
>
> **优点**
>
> **查询数据通过索引定位,查询任意数据耗时相同,`查询效率高`**
>
> ----
>
> **缺点**
>
> **删除数据时,要将原始数据删除,同时后面毎个数据前移,`删除效率低` ** <br>
> **添加数据时,添加位置后的每个数据后移,再添加元素,`添加效率极低`**
>
> <br>**由此可见 数组是一种查询快,增删慢的模型**

----





----





## 数据结构之【链表】

----

> ![链表](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/链表.png)
> ![链表_1](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/链表_1.png)
>
> **优点**
>
> **链表是一种`增删快`的模型(对比数组)**
>
> **缺点**
>
> **链表是一种`查询慢`的模型(对比数组)**

----







## 数据类型之【哈希表】

----

> **JDK8之前,底层采用 [数组] + [链表] 实现,可以说是一个元素为链表的数组。**<br>
> **JDK8以后,在长度比较长的时候,底层实现了优化。**
>
> ![Java哈希表](https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Java%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB.assets/Java哈希表.png)

----





# 🌸集合案例

----





## 斗地主


```java
package 基础知识.集合.集合案例;

import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;

public class DouDiZu {

    private String userName1 = "尴尬了";
    private String userName2 = "尴尬帝";
    private String userName3 = "尴尬酱";

    //启动
    public static void main(String[] args) {
        new DouDiZu().init();
    }

    //初始化
    public void init() {

        //创建牌盒，使用ArrayList数组
        ArrayList<String> list = new ArrayList<>();

        //创建花色组
        String[] colors = {"方块", "红桃", "黑桃", "红桃"};
        //创建牌点数
        String[] nums = {"2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"};

        //将num添加到colors中
        for (String color : colors) {
            for (String num : nums) {
                list.add(color + num);
            }
        }
        //添加大小王
        list.add("小王");
        list.add("大王");

        //进行洗牌使用Collections的shuffle()静态方法
        Collections.shuffle(list);

        //创建3数组，分别存储3个人的牌数
        ArrayList<String> user1 = new ArrayList<>();
        ArrayList<String> user2 = new ArrayList<>();
        ArrayList<String> user3 = new ArrayList<>();
        ArrayList<String> dp = new ArrayList<>();

        //分牌，使用普通for循环
        for (int i = 0; i < list.size(); i++) {
            String str = list.get(i);
            if (i >= list.size() - 3) {
                dp.add(str);
            } else if (i % 3 == 0) {
                user1.add(str);
            } else if (i % 3 == 1) {
                user2.add(str);
            } else if (i % 3 == 2) {
                user3.add(str);
            }
        }
        //进行对牌的排序
        HashMap<String, Integer> map = new HashMap<>();



        //调用查看牌的方法
        Value(userName1, user1);
        Value(userName2, user2);
        Value(userName3, user3);
        Value("底牌数", dp);


    }

    //对用户的牌排序
    public static void sort(String userName,ArrayList<String> list){
        HashMap<String, String> map = new HashMap<>();
        //利用Map集合存储牌和牌的个数

    }

    //查看牌数的方法。
    public static void Value(String name, ArrayList<String> array) {
        System.out.println("姓名：" + name);
        System.out.print("牌为：");
        for (String str : array) {
            System.out.print(str + " ");
        }
        System.out.println("\n======================================================================================");
    }

}
```



----





## New斗地主


```java
package 基础知识.集合.集合案例;
/*
 * 存储使用 HashMap
 * 洗牌使用 ArrayList
 * 用户呈现 TreeSet
 *
 * */

import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.TreeSet;

public class DouDiZuNew {

    //创建用户信息
    private String userName1 = "尴尬了";
    private String userName2 = "尴尬帝";
    private String userName3 = "尴尬酱";
    //创建牌色和牌数
    private String[] colorOf = {"方块", "红桃", "黑桃", "红桃"};
    private String[] numOf = {"2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"};
    //用户的牌,   创建用户牌容器, 使用TreeSet集合，进行排序，使用自然排序(升序)
    private TreeSet<Integer> user1 = new TreeSet<>();
    private TreeSet<Integer> user2 = new TreeSet<>();
    private TreeSet<Integer> user3 = new TreeSet<>();
    private TreeSet<Integer> hands = new TreeSet<>();


    /**
     * 启动
     */
    public static void main(String[] args) {
        //启动
        new DouDiZuNew().init();
    }

    /**
     * 初始化
     */
    public void init() {

        //创建HashMap集合
        HashMap<Integer, String> map = new HashMap<>();

        //创建牌容器,这里存储的是HashMap集合中的键值 index
        ArrayList<Integer> list = new ArrayList<>();

        /**操作数组：*/
        //调用存储牌，存储编号
        this.hashMapAdd(map, list);

        //调用洗牌
        this.shuffle(list);

        //调用发牌
        this.distribute(list, map);
        System.out.println(list);

        //调用查看用户牌
        look(userName1, user1, map);
        look(userName2, user2, map);
        look(userName3, user3, map);

    }

    /**
     * ================================================================================
     */

    //向HashMap中存储牌数 , 并且向ArrayList集合当中添加键值编号
    public void hashMapAdd(HashMap<Integer, String> map, ArrayList<Integer> list) {
        //初始化点数
        int index = 0;
        //先是numOf进行循环, 保证TreeSet可以正常排序。
        for (String num : numOf) {
            for (String color : colorOf) {
                //向map中添加键值对
                map.put(index, color + num);
                //向ArrayList添加键值编号
                list.add(index);
                //index++
                index++;
            }
        }
        //添加大小王
        map.put(index, "小王");
        list.add(index);
        index++;
        map.put(index, "大王");
        list.add(index);
    }

    //重新洗牌
    public void shuffle(ArrayList<Integer> list) {
        Collections.shuffle(list);
    }

    //发牌的方法
    public void distribute(ArrayList<Integer> list, HashMap<Integer, String> map) {
        for (int i = 0; i < list.size(); i++) {
            if (i >= 54) {
                hands.add(list.get(i));
            } else if (i % 3 == 0) {
                user1.add(list.get(i));
            } else if (i % 3 == 1) {
                user2.add(list.get(i));
            } else if (i % 3 == 2) {
                user3.add(list.get(i));
            }
        }
    }

    //查看牌 , 用户呈现排序
    public void look(String name, TreeSet<Integer> set, HashMap<Integer, String> map) {
        System.out.println("姓名:" + name);
        System.out.print("牌数为:");
        int in = 1;
        for (Integer index : set) {
            if (in < set.size()) {
                System.out.print(map.get(index) + " , ");
            } else {
                System.out.print(map.get(index + "。"));
            }
            in++;
        }
        System.out.println();
        System.out.println("=====================================================================");
    }
}
```



----



# 🌸单词复习



|  **单词/引用**  | **翻译/解释**  |
| :-------------: | :------------: |
|                 |                |
| **Collection**  |                |
|   **remove**    |    **删除**    |
|    **clear**    |    **清除**    |
|  **contains**   |    **存在**    |
|   **isEmpty**   |   **is为空**   |
|                 |                |
|  **Iterator**   |   **迭代器**   |
|   **hasNext**   |   **下一个**   |
|                 |                |
| **LinkedList**  |                |
|    **First**    |    **开头**    |
|    **Last**     |    **末尾**    |
|                 |                |
| **Compatable ** |  **自然排序**  |
| **Comparator ** | **比较器排序** |
|  **compare **   |    **比较**    |
|                 |                |
|     **Map**     |   **地图 **    |
|    **Key **     |   **键   **    |
|    **Value**    |     **值**     |
|     **put**     | **添加元素  ** |
|  **entrySet **  | **便利使用 **  |
|                 |                |
|                 |                |
|                 |                |















