---
title: Java的Stream流👻复习
date: 2021-06-16 09:50:34
tags: ["后端","Java基础","Stream流"]
cover: "https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/img/blog-top/stream.png"
categories:
  - 后端
  - Java
  - Java基础
---



## 🌸Stream总复习





## 代码-初步体验Stream流

```java
package 基础知识.λ和Stream流.A1_初步体验Stream流;

import java.util.ArrayList;
import java.util.function.Predicate;

public class TestStream {

    public static void main(String[] args) {

        //实现该方法1: 不使用Stream流
        ArrayList<String> list = new ArrayList<>();
        list.add("尴尬");
        list.add("尴尬了");
        list.add("尴尬酱");
        list.add("这就尴尬了");
        list.add("尴尬帝");
        list.add("不尴尬");
        list.add("九监九介");
        //第一次筛选: 以[尴]开头的
        ArrayList<String> list1 = new ArrayList<>();
        for (String s : list) {
            if (s.startsWith("尴")) {
                list1.add(s);
            }
        }
        //第二次筛选: 字符长度为三个字符
        ArrayList<String> list2 = new ArrayList<>();
        for (String s : list1) {
            if (s.length() == 3) {
                list2.add(s);
            }
        }
        //打印输出
        System.out.println(list2);
        System.out.println("==========================");
        
        //====================================================================================

        //实现该方法2: 使用Stream流
        ArrayList<String> listStream = new ArrayList<>();
        listStream.add("尴尬");
        listStream.add("尴尬了");
        listStream.add("尴尬酱");
        listStream.add("这就尴尬了");
        listStream.add("尴尬帝");
        listStream.add("不尴尬");
        listStream.add("九监九介");

        //只要一个步骤操作 简化了上面所有代码
        listStream.stream().filter( s -> s.startsWith("尴") )
            .filter(s -> s.length() == 3)
            .forEach( s-> System.out.println(s) );

        System.out.println("==========================");

        //优化: 方法引用
        listStream.stream().filter(s -> s.startsWith("尴"))
                     .filter(s -> s.length()==3)
                     .forEach(System.out::println);


    }

}
```





## Sttream流的一生



![Stream流的生成方式](
https://ayaka-icu-oss.oss-cn-beijing.aliyuncs.com/blog/Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F.assets/Stream%E6%B5%81%E7%9A%84%E7%94%9F%E6%88%90%E6%96%B9%E5%BC%8F.png)



代码

```java
package 基础知识.λ和Stream流.A2_Stream流的生成方式;
/*
    Stream流的常见生成方式:
        1: Collection集合体系 可以使用默认方法stream()生成流
                default Stream<E> stream(){//...}

        2: Map集合体系 不能直接生成 只能间接的生成流

        3: 数组 可以通过Stream接口的静态方法:
                of(T... values)生成流

*/


import java.util.*;
import java.util.stream.*;

public class TestNewStream {

    public static void main(String[] args) {
        //1: Collection集合体系 可以使用默认方法stream()生成流
        //      default Stream<E> stream(){//...}
        List<String> list = new ArrayList<>();
        Stream<String> listStream = list.stream();

        Set<String> set = new HashSet<>();
        Stream<String> setStream = set.stream();

        //2: Map集合体系 不能直接生成 只能间接的生成流
        Map<String,Integer> map =new HashMap<>();
        //通过map的 [键] 和 [值] 来生成Stream流
        Stream<String> mapKeyStream = map.keySet().stream();
        Stream<Integer> mapValueStream = map.values().stream();

        //通过泛型类型为Map集合
        //Stream<Map<String,Integer>> sm =map.entrySet().stream();

        //3: 数组 可以通过Stream接口的静态方法:
        //      of(T... values)生成流
        String[] strArray1 = {"永远","爱你"};
        Stream<String> strStream1 = Stream.of(strArray1);
        Stream<String> strStream2 = Stream.of("永远","爱你");
        Stream<Integer> intStream = Stream.of(1,2,3);//可变参数，可以直接写个数组，也可以单独写数值

    }
}
```





## Stream常见的中间操作



- `Stream<T> filter(Predicate predicate)`: 用于对流中的数据进行过滤
  - Predicate接口中的方法
  - `boolean test(T t)`:对给定的参数进行判断，返回一个布尔值
- `Stream<T> limit(long maxSize)`: 返回此流中的元素组成的流，截取前指定参数个数的数据
- `Stream<T>skip(long n)`: 跳过指定参数个数的数据，返回由该流的剩余元素组成的流
- `Static<T> Stream<T> concat(Stream a, Stream b)`: 合并a和b两个流为一个流
- `Stream<T> distinct()`: 返回由该流的不同元素（根据Object.equals(Object))组成的流
- `Stream<T> sorted()`: 返回由此流的元素组成的流，根据自然顺序排序
- `Stream<T> sorted(Comparator comparator)`: 返回由该流的元素组成的流，根据提供的Comparatori进行排序
- `<R> Stream<R> map(Function mapper)`: 返回由给定函数应用于此流的元素的结果组成的流
  - Function接口中的方法
  - R apply(Tt)
- IntStream mapTolnt(TolntFunction mapper)`: 返回一个IntStream其中包含将给定函数应用于此流的元素的结果
  - IntStream: 表示原始int流
  - TolntFunction接口中的方法
  - int applyAsInt(Tvalue)



### filter 方法

```java
package 基础知识.λ和Stream流.A3_Stream常见的中间操作;
/*
 Stream中的filter过滤器
    Stream<T> filter(Predicate<? super T> predicate)
    返回由与此给定谓词匹配的此流的元素组成的流。

    使用Lambda表达式
*/

import java.util.ArrayList;
import java.util.stream.Stream;

public class Test1_filter_Me {

    public static void main(String[] args) {

        //先创建一个测试数组
        ArrayList<String> list = new ArrayList<>();
        list.add("尴尬");
        list.add("尴尬酱");
        list.add("尴尬帝");
        list.add("这就尴尬");
        list.add("不尴尬");

        //需求1: 把list集合中元素开头为[尴]的筛选出来
        //Stream<T> filter(Predicate<? super T> predicate);
        /*
        list.stream().filter( (String s) ->{
            return s.startsWith("尴");//返回值是一个布尔值。
        } ).forEach(s->System.out.println(s));
        */

        //Lambda表达式优化简化
        list.stream().filter(s -> s.startsWith("尴"))//λ省略
                .forEach(System.out::println);//类方法引用

        System.out.println("============================");

        //需求2: 将长度为3的字符筛选出来
        list.stream().filter(s -> s.length() == 3)//筛选长度为3的字符
                .forEach(System.out::println);

        System.out.println("============================");

        //需求3: 开头为[尴],长度为3的字符筛选出来
        list.stream().filter(s -> s.startsWith("尴"))
                     .filter(s -> s.length() == 3)
                     .forEach(System.out::println);

        list.stream().limit(list.size()).forEach(System.out::println);

    }
}
```



----



### limit && skip

```java
package 基础知识.λ和Stream流.A3_Stream常见的中间操作;
/*
Stream流中间操作：

    Stream<T> limit(long maxSize)
        返回由该流的元素组成的流，截断长度不能超过maxSize 。

    Stream<T> skip(long n)
        在丢弃流的第一个n元素后，返回由该流的n元素组成的流。
        如果此流包含少于n元素，那么将返回一个空流。和


*/

import java.util.ArrayList;

public class Test2_limit_skip_Me {

    public static void main(String[] args) {

        //先创建一个测试数组
        ArrayList<String> list = new ArrayList<>();
        list.add("尴尬");
        list.add("尴尬酱");
        list.add("尴尬帝");
        list.add("这就尴尬");
        list.add("不尴尬");

        //使用limit限制长度   //这个参数是 [元素个数] 不是索引
        list.stream().limit(3).forEach(System.out::println);

        System.out.println("===========================");

        //可以进行便利        //size方法也是 [元素个数] 不是索引
        list.stream().limit(list.size()).forEach(System.out::println);


        System.out.println("===========================");
        System.out.println("===========================");

        //使用skip跳过前参数个元素 参数类型为long
        //然后截取以后的所有元素。
        list.stream().skip(3).forEach(System.out::println);


        System.out.println("===========================");
        System.out.println("===========================");

        //跳过前两个，将剩下的前两个的输出
        list.stream().skip(2).limit(2).forEach(System.out::println);


    }

}
```



----



### concat && distinct

```java
package 基础知识.λ和Stream流.A3_Stream常见的中间操作;
/*
Stream流常用的方法:

    static <T> Stream<T> concat(Stream<? extends T> a,
                            Stream<? extends T> b)
    创建一个懒惰连接的流，其元素是第一个流的所有元素，后跟第二个流的所有元素。
    如果两个输入流都被排序，则生成的流被排序，并且如果任何一个输入流是并行的，则并行。
    当结果流关闭时，调用两个输入流的关闭处理程序。

    Stream<T> distinct()
        返回由该流的不同元素（根据Object.equals(Object) ）组成的流。


*/

import java.util.ArrayList;
import java.util.stream.Stream;

public class Test3_concat_distinct {

    public static void main(String[] args) {

        //先创建一个集合
        ArrayList<String> list = new ArrayList<>();
        list.add("尴尬");
        list.add("尴尬酱");
        list.add("尴尬帝");
        list.add("这就尴尬");
        list.add("不尴尬");

        //要求1: 获取前3个元素的流
        Stream<String> s1 = list.stream().limit(3);

        //要求2: 获取跳过2个元素的流
        Stream<String> s2 = list.stream().skip(2);

        //要求3: 合并要求1和2的流,并输出
        Stream.concat(s1,s2).forEach(System.out::println);

        System.out.println("============");
        s1 = list.stream().limit(3);
        s2 = list.stream().skip(2);

        //要求4: 合并要求1和2的流,并输出,元素不重复
        Stream.concat(s1,s2).distinct().forEach(System.out::println);
        //注意: Stream.concat(s1,s2)要求3用过后,s1和s2不能再次使用,要重新赋值。

    }

}
```



---



### sorted 方法

```java
package 基础知识.λ和Stream流.A3_Stream常见的中间操作;
/*
Stream流常见的中间操作

    //自然排序
    Stream<T> sorted()
        返回由此流的元素组成的流，根据自然顺序排序。
        如果该流的元件不是Comparable ，
        一个java.lang.ClassCastException执行终端操作时，可以抛出。

    //比较器排序
    Stream<T> sorted(Comparator<? super T>  comparator)
        返回由该流的元素组成的流，根据提供的Comparator进行排序。
        对于有序流，排序稳定。 对于无序的流，不能保证稳定性。

*/

import java.util.ArrayList;

public class Test4_sorted {

    public static void main(String[] args) {

        //先创建一个集合
        ArrayList<String> list = new ArrayList<>();
        list.add("abc");
        list.add("abcd");
        list.add("da");
        list.add("bac");
        list.add("zzzz");
        list.add("hhhh");
        list.add("dcba");
        list.add("z");

        //要求1: 自然排序,按照字母顺序排序
        list.stream().sorted().forEach(System.out::println);

        System.out.println("========");

        //要求2: 比较器排序,按照字符长度进行排序
        list.stream().sorted((s1,s2)->s1.length()-s2.length()).forEach(System.out::println);

        System.out.println("========");

        //要求3: 比较器排序,先按照字符长度排序,如果字符长度相同,则按照字符顺序进行排序。
        list.stream().sorted((s1,s2)->{
            int num1 = s1.length() - s2.length();
            int num2 = num1==0? s1.compareTo(s2) : num1;
            return num2;
        }).forEach(System.out::println);

    }
}
```



---



### map 方法

```java
package 基础知识.λ和Stream流.A3_Stream常见的中间操作;
/*
Stream流中常见的中间操作:

    map<R> Stream<R> map(Function<? super T,? extends R> mapper)
        返回由给定函数应用于此流的元素的结果组成的流。
        这是一个intermediate operation 。

    IntStream mapToInt(ToIntFunction<? super T> mapper)
        返回一个IntStream ，其中包含将给定函数应用于此流的元素的结果。
        这是一个intermediate operation 。

*/

import java.util.ArrayList;
import java.util.stream.IntStream;

public class Test5_Map {

    public static void main(String[] args) {

        //先创建一个数组
        ArrayList<String> list = new ArrayList<>();
        list.add("10");
        list.add("20");
        list.add("30");
        list.add("40");
        list.add("50");

        //将字符转为int  map
        //list.stream().map(s -> Integer.parseInt(s)).forEach(System.out::println);
        //改进
        list.stream().map(Integer::parseInt).forEach(System.out::println);

        System.out.println("=========");

        //将字符转为int,并求和  mapToInt
        IntStream intStream = list.stream().mapToInt(Integer::parseInt);
        //调用了mapToInt()方法后,返回的IntStream类型。
        int sum = intStream.sum();//IntStream类中有自己的方法。
        System.out.println(sum);

        //改进
        int sum1 = list.stream().mapToInt(Integer::parseInt).sum();
        System.out.println(sum1);

    }
}
```





---





## Stream终结操作

Streami流的常见终结操作方法


- `void forEach(Consumer action)`: 对此流的每个元素执行操作
  - Consumer接口中的方法
  - `void accept(T t)`: 对给定的参数执行此操作
- `long count()`: 返回此流中的元素数





----





## Stream流的收集方法

```java
package 基础知识.λ和Stream流.A5_Stream流收集操作;
/*
Stream流的收集方法:

    R collect (Collector coll)

    其中: Collectors提供了具体的书记方式。
        public static <T> Collector toList(): 把元素收集到List集合中。
        public static <T> Collector toSet():  把元素手机到Set集合中
        public static Collector toMap (Function keyMapper, Function valueMapper);

*/

import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class TestCollect {

    public static void main(String[] args) {

        listMe();
        setMe();
        mapMe();
        Me();


    }

    //set集合
    public static void listMe() {
        //先创建一个list集合
        ArrayList<String> list = new ArrayList<>();
        list.add("尴尬酱");
        list.add("这就尴尬了");
        list.add("尴尬");
        list.add("尴尬帝");
        list.add("尴尬了");
        list.add("不尴尬");

        //需求1: 得到名字为3个字符的流
        Stream<String> listStream = list.stream().filter(s -> s.length() == 3);

        //需求2: 使用Stream流操作完毕的数据收集到List集合中并便利
        List<String> list1 = listStream.collect(Collectors.toList());
        for (String str : list1) {
            System.out.println(str);
        }

        System.out.println("===================================");

    }


    //set集合
    public static void setMe() {
        //创建Set集合
        HashSet<Integer> set = new HashSet<>();
        set.add(10);
        set.add(15);
        set.add(20);
        set.add(35);
        set.add(55);
        set.add(66);

        //需求1: 得到年龄为30以上的流
        Stream<Integer> setStream = set.stream().filter(i -> i > 30);

        //需求2: 使用Stream流操作完毕的数据收集到Set集合中并便利
        Set<Integer> setNew = setStream.collect(Collectors.toSet());
        for (int i : setNew) {
            System.out.println(i);
        }

        System.out.println("===================================");
    }


    //数组
    public static void mapMe() {
        //创建一个字符数组
        String[] array = {"尴尬酱,16", "尴尬帝,20,", "尴尬了,9", "尴尬,18", "不尴尬,13"};

        //需求1: 得到字符串中年龄大于15岁的流
        Stream<String> arrayStream =
                Stream.of(array).filter(s -> Integer.parseInt(s.split(",")[1]) > 15);

        //需求2: 将得到的字符串 按姓名为键 年龄为值 放入map集合当中
        Map<String,Integer> map =arrayStream.collect(Collectors.toMap( //通过Collectors中的toMap获取Collector类对象
                s -> s.split(",")[0],//第一个Lambda为键 自动识别泛型
                s -> Integer.parseInt(s.split(",")[1])));//第二个Lambda为值 这里转为int类型
        //遍历
        Set<String> mapK = map.keySet();
        for (String k : mapK) {
            int v = map.get(k);
            System.out.println("姓名: " + k + ", 年龄: " + v);
        }
    }

    public static void Me() {
    }

}
```



----



## 单词复习



|   **单词**   |  **解释**  |
| :----------: | :--------: |
|              |            |
|  **Stream**  |   **流**   |
|              |            |
|  **filter**  |  **过滤**  |
|              |            |
|  **limit**   |  **限制**  |
|              |            |
|   **skip**   |  **跳过**  |
|              |            |
|  **concat**  |  **合并**  |
|              |            |
| **distinct** | **不同的** |
|              |            |
|  **sorted**  |  **分类**  |
|              |            |
|              |            |
|              |            |



