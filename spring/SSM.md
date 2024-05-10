# SSM三大框架复习





## Spring框架



### 介绍

---

![image-20220413082758419](../assets/md-img/SSM.assets/image-20220413082758419.png)

---

<br>

**Spring优势**

---

![image-20220413082944652](../assets/md-img/SSM.assets/image-20220413082944652.png)

![image-20220413083028235](../assets/md-img/SSM.assets/image-20220413083028235.png)

---



**Spring体系结构**

---

![image-20220413083135688](../assets/md-img/SSM.assets/image-20220413083135688.png)

---

---



### Spring开发步骤

![image-20220413083256330](../assets/md-img/SSM.assets/image-20220413083256330.png)



#### 一. 坐标 导入基本包

```xml
<dependencies>
    
	<!--导入spring的context坐标，context依赖core、beans、expression-->
	<dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.18</version>
    </dependency>
    
</dependencies>

```

---



#### 二. 编写Dao和实现类对象

```java
public interface UserDao {
	public void save();
}


public class UserDaoImpl implements UserDao {
	@Override
	public void save() {
	System.out.println("UserDao save method running....");
	}
}

```

---



#### 三. 核心配置文件 applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">


</beans>
```

---



#### 四. 在 Spring 配置文件中配置 UserDaoImpl

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"></bean>
    
</beans>
```

---



#### 五. 使用Spring的API获得Bean实例

```java
@Test
public void test1(){
    
	ApplicationContext applicationContext = new
        ClassPathXmlApplicationContext("applicationContext.xml");
	UserDao userDao = (UserDao) applicationContext.getBean("userDao");
	userDao.save();
    
}
```

---





### Spring配置文件



#### Bean标签

----



##### Bean标签基本配置

![image-20220413084939770](../assets/md-img/SSM.assets/image-20220413084939770.png)

---



##### **Scope范围配置**

![image-20220413085143163](../assets/md-img/SSM.assets/image-20220413085143163.png)

1. 当scope的取值为**`singleton`**时 
   1. Bean的实例化个数：**1个 **
   2. Bean的实例化时机：当Spring核心文件被加载时，实例化配置的Bean实例 
   3. Bean的生命周期： 
      1. 对象创建：当应用加载，**创建容器时**，对象就被创建了
      2. 对象运行：只要容器在，对象一直活着 
      3. 对象销毁：当应用卸载，销毁容器时，**对象就被销毁了**
2. 当scope的取值为**`prototype`**时
   1. Bean的实例化个数：**多个**
   2. Bean的实例化时机：当调用getBean()方法时实例化Bean
      1. 对象创建：当使用对象时，**创建新的对象实例**
      2. 对象运行：只要对象在使用中，就一直活着 
      3. 对象销毁：当对象长时间不用时，**被 Java 的垃圾回收器回收了**

---



##### Bean生命周期配置

<font color="red">**init-method：指定类中的初始化方法名称**</font>

<font color="red">**destroy-method：指定类中销毁方法名称**</font>

---



##### Bean实例化三种方式

<font color="red">	**1 无参构造方法实例化 **</font>

<font color="red">	**2 工厂静态方法实例化 **</font>

<font color="red">	**3 工厂实例方法实例化**</font>



1. **无参构造方法实例化**

```xml
<bean id="userDao" class="com.itheima.factory.StaticFactoryBean"/>
<!-- 没有无参构造时不能被创建 -->
```





2. **工厂静态方法实例化**

<font color="red">factory-method 寻找该id的静态方法 从返回值中获取对象</font>

```java
public class StaticFactoryBean {
	public static UserDao createUserDao(){
		return new UserDaoImpl();
	}
}

```

```xml
<bean id="userDao" class="com.itheima.factory.StaticFactoryBean" 
factory-method="createUserDao"/>
```



3. **工厂实例方法实例化**

<font color="red">factory-bean 寻找此id的bean对象 </font>

<font color="red">factory-method 寻找到的bean对象的id</font>

```java
public class DynamicFactoryBean {
	public UserDao createUserDao(){
		return new UserDaoImpl();
	}
}
```

```xml
<bean id="factoryBean" class="com.itheima.factory.DynamicFactoryBean"/>
<bean id="userDao" factory-bean="factoryBean" factory-method="createUserDao"/>
```

----





##### 依赖注入



###### 注入方式

![image-20220413091857306](../assets/md-img/SSM.assets/image-20220413091857306.png)

![image-20220413091917185](../assets/md-img/SSM.assets/image-20220413091917185.png)





<font color="red">**Bean的依赖注入概念：**</font>



**依赖注入（<font color="red">Dependency Injection</font>）：它是 Spring 框架核心 IOC 的具体实现。** 



**在编写程序时，通过控制反转，把对象的创建交给了 Spring，但是代码中不可能出现没有依赖的情况。 IOC 解耦只是降低他们的依赖关系，但不会消除。例如：业务层仍会调用持久层的方法。 **



**那这种业务层和持久层的依赖关系，在使用 Spring 之后，就让 Spring 来维护了。 简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取。**



![image-20220413092516763](../assets/md-img/SSM.assets/image-20220413092516763.png)



**Set方式:**

```java
public class SetServiceImpl implements SetService {
    
	private SetDao setDao;
    
    public void setUserDao(SetDao setDao) {
		this.setDao = setDao;
	}
    
	@Override
	public void save() {
		setDao.save();
	}
    
}
```

```xml
<bean id="setDao" class="com.ganga.dao.impl.SetDaoImpl"></bean>
<!-- 使用property标签 -->
<bean id="setService" class="com.ganga.service.impl.SetServiceImpl">
	<property name="setDao" ref="setDao"/>
</bean>

<!--
      在setService中添加依赖关系 将setDao注入到setService
          set方法注入

      添加标签 property
              name: 该class(com.ganga.service.impl.SetServiceImpl)
                      的用于获取对象的 set...方法
                      setSetDao(SetDao setDao)方法 去掉set
                      并将第一个字母变为小写
                    即: ref="setDao"
              ref:  注入对象的bean id标识
                    即: name="setDao"
-->
```

另外还可以引入<font color="red">**p**</font>标签

``p:setDao-ref="setDao"``

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:p="http://www.springframework.org/schema/p"

       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
        第一步: 在命名空间中导入p标签
                 xmlns:p="http://www.springframework.org/schema/p"
    -->


    <bean id="setDao" class="com.ganga.dao.impl.SetDaoImpl"></bean>

    <bean id="setService" class="com.ganga.service.impl.SetServiceImpl" p:setDao-ref="setDao"></bean>
    <!--
        第二步: 在setService中 设置 p:setDao-ref="setDao"

                p:后面加 对应的set方法 如:property 的 name属性
                -ref 是指set方式注入对象  [不带ref是注入的属性]
                ="注入对象的bean 的id" 如property 的 ref属性
    -->

</beans>
```

---



**构造方法注入**

```java
public class ConstructorServiceImpl implements ConstructorService {

    //创建ConstructorDao的空对象
    private ConstructorDao constructorDao;

    //无参构造方法
    public ConstructorServiceImpl(){

    }

    //有参构造
    public ConstructorServiceImpl(ConstructorDao constructorDao){
        //将参数放入constructorDao对象当中
        this.constructorDao = constructorDao;
    }

    @Override
    public void method() {
        //最后调用方法
        constructorDao.method();
    }


}
```

```xml

<bean id="constructorDao" class="com.ganga.dao.impl.ConstructorDaoImpl"></bean>

<bean id="constructorService" class="com.ganga.service.impl.ConstructorServiceImpl">
	<constructor-arg name="constructorDao" ref="constructorDao"/>
</bean>
    <!--
        在setService中添加依赖关系 将setDao注入到setService
            构造方法注入

        添加标签 constructor-arg
                    name: 指定的构造器参数名称
                    ref:  引用容器当中bean id名

    -->
```

---





###### Bean依赖注入的数据类型

上面的操作，都是注入的引用Bean，处了对象的引用可以注入，普通数据类型，集合等都可以在容器中进行注入。 

注入数据的三种数据类型

<font color="red">**普通数据类型**</font>

<font color="red">**引用数据类型**</font>

<font color="red">**集合数据类型** </font>

其中引用数据类型，此处就不再赘述了，之前的操作都是对UserDao对象的引用进行注入的，下面将以set方法注入为 例，演示普通数据类型和集合数据类型的注入。





<font color="red">**普通数据类型**</font>

```xml
<!-- 以set方式注入 -->
<bean id="x01SetDao" class="com.ganga.dao.impl.X01SetDaoImpl">
    <property name="username" value="尴尬酱"/>
    <property name="age" value="9"/>
</bean>

<!-- 以构造器方式注入 -->
<bean id="x01ConstructorDao" class="com.ganga.dao.impl.X01ConstructorDaoImpl">
    <constructor-arg name="username" value="这就尴尬了"/>
    <constructor-arg name="age" value="20"/>
</bean>
```



<font color="red">**引用数据类型**</font>



<font color="red">**集合数据类型** </font>

```java
public class X03DaoImpl implements X03Dao {

    //注入List集合
    private List<String> stringList;
    private List<User> userList;
    //注入Map集合
    private Map<String, User> stringUserMap;
    //注入Properties
    private Properties properties;

    //注入List集合 set方法
    public void setStringList(List<String> stringList) {
        this.stringList = stringList;
    }
    public void setUserList(List<User> userList) {
        this.userList = userList;
    }

    //注入Map集合 set方法
    public void setStringUserMap(Map<String, User> stringUserMap) {
        this.stringUserMap = stringUserMap;
    }

    //注入Properties set方法
    public void setProperties(Properties properties) {
        this.properties = properties;
    }

    @Override
    public void method() {
        System.out.println("====== List ======");
        System.out.println(stringList);
        for (User user : userList) {
            System.out.println(user);
        }

        System.out.println("\n====== Map ======");
        System.out.println(stringUserMap);

        System.out.println("\n====== Properties ======");
        System.out.println(properties);

        System.out.println("\nrun X03DaoImpl method...");
    }
}
```

applicationContext.xml

```xml
<bean id="x03Dao" class="com.ganga.dao.impl.X03DaoImpl">

        <!--注入List数据-->
        <property name="stringList">
            <list>
                <value>亚索哥哥</value>
                <value>永恩弟弟</value>
                <value>卡特妹妹</value>
            </list>
        </property>
        <property name="userList">
            <list>
                <ref bean="user1"/>
                <ref bean="user2"/>
                <ref bean="user3"/>
                <bean class="com.ganga.pojo.User">
                    <constructor-arg name="username" value="盖伦哥哥"/>
                    <constructor-arg name="age" value="10"/>
                    <constructor-arg name="adds" value="德玛西亚"/>
                </bean>
            </list>
        </property>

        <!--Map集合注入-->
        <property name="stringUserMap">
            <map>
                <entry key="u1" value-ref="user1"/>
                <entry key="u2" value-ref="user2"/>
                <entry key="u3" value-ref="user3"/>
            </map>
        </property>

        <!--Properties注入-->
        <property name="properties">
            <props>
                <prop key="p1">亚索哥哥</prop>
                <prop key="p2">永恩哥哥</prop>
                <prop key="p3">卡特妹妹</prop>
            </props>
        </property>

    </bean>

    <!--user对象-->
    <bean id="user1" class="com.ganga.pojo.User">
        <constructor-arg name="username" value="亚索哥哥"/>
        <constructor-arg name="age" value="18"/>
        <constructor-arg name="adds" value="艾欧尼亚"/>
    </bean>
    <bean id="user2" class="com.ganga.pojo.User">
        <constructor-arg name="username" value="永恩哥哥"/>
        <constructor-arg name="age" value="16"/>
        <constructor-arg name="adds" value="艾欧尼亚"/>
    </bean>
    <bean id="user3" class="com.ganga.pojo.User">
        <constructor-arg name="username" value="卡特妹妹"/>
        <constructor-arg name="age" value="9"/>
        <constructor-arg name="adds" value="诺克萨斯"/>
    </bean>

```



######  引入其他配置文件（分模块开发）

实际开发中，Spring的配置内容非常多，这就导致Spring配置很繁杂且体积很大，所以，可以将部分配置拆解到其他 配置文件中，而在Spring主配置文件通过import标签进行加载

```XML
<import resource="applicationContext-xxx.xml"/>
```



---



### **Spring相关API**



#### ApplicationContext的继承体系

<font color="red">**applicationContext：**</font>接口类型，代表应用上下文，可以通过其实例获得 Spring 容器中的 Bean 对象



![ApplicationContext继承体系](../assets/md-img/SSM.assets/ApplicationContext继承体系.png)



---



#### ApplicationContext的实现类



1. <font color="red">**ClassPathXmlApplicationContext**</font>

   ​	它是从类的根路径下加载配置文件 推荐使用这种

2. <font color="red">**FileSystemXmlApplicationContext**</font>

   ​	它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。

3. <font color="red">**AnnotationConfigApplicationContext**</font>

   ​	当使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。

---



#### getBean()方法使用



常用的有两个

1. 通过bean的 字符id来找 ``getBean("userDao")``

   其中，当参数的数据类型是字符串时，表示根据Bean的id从容器中获得Bean实例，返回是Object，需要强转。

   

2. 通过类的calss自动匹配  ``getBean(UserDao.class)``

   当参数的数据类型是Class类型时，表示根据类型从容器中匹配Bean实例，当容器中相同类型的Bean有多个时， 则此方法会报错。



---





### Spring配置数据源



#### 数据源（连接池）的作用

1. 数据源(连接池)是提高程序性能如出现的
2. 事先实例化数据源，初始化部分连接资源
3. 使用连接资源时从数据源中获取
4. 使用完毕后将连接资源归还给数据源



常见的数据源(连接池)：<font color="red">**DBCP、C3P0、BoneCP、Druid**</font>等



#### 数据源的开发步骤

1. 导入数据源的坐标和数据库驱动坐标
2. 创建数据源对象
3. 设置数据源的基本连接数据
4. 使用数据源获取连接资源和归还连接资源



#### Spring配置数据源

可以将DataSource的创建权交由Spring容器去完成

​	DataSource有无参构造方法，而Spring默认就是通过无参构造方法实例化对象的 

​	DataSource要想使用需要通过set方法设置数据库连接信息，而Spring可以通过set方法进行字符串注入



**抽取jdbc配置文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 先引入context命名空间 和 约束路径 -->

    <!-- 通过Spring加载properties的方法 -->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <bean id="c3p0DataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driverClass}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.user}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    
</beans>
```

```properties
# jdbc.properties
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///webcase?useSSL=false&useServerPrepStmts=true
jdbc.user=root
jdbc.password=ganga
```



<font color="red">**重点: Spring容器加载properties文件**</font>

```xml
<!-- 通过Spring加载properties的方法 -->
<context:property-placeholder location="xx.properties"/>
 <!-- 可以使用el表达式获取properties文件的数据 -->
<property name="" value="${key}"/>
```

---





### Spring注解开发



Spring是轻代码而重配置的框架，配置比较繁重，影响开发效率，所以注解开发是一种趋势，注解代替xml配置 文件可以简化配置，提高开发效率。



#### Spring原始注解



<font color="red">**Spring原始注解主要是替代的配置**</font>

![原注解](../assets/md-img/SSM.assets/原注解.png)



<font color="red">**注意： **</font>

​		使用注解进行开发时，需要在applicationContext.xml中配置组件扫描，作用是指定哪个包及其子包下的Bean 需要进行扫描以便识别使用注解配置的类、字段和方法。

```xml
<!--注解的组件扫描-->
<context:component-scan base-package="com.ganga"></context:component-scan>
<!-- 扫描com.ganga包及其以下包的的文件 -->
```



使用@Component或@Service标识UserServiceImpl需要Spring进行实例化

使用@Autowired或者@Autowired+@Qulifier或者@Resource进行userDao的注入

使用@Value进行字符串的注入  一般配合el表达式来体现解耦合

```java
//添加bean id="annoFirstService"
//Service层 用 @Service
@Service("annoFirstService")
public class AnnoFirstServiceImpl implements AnnoFirstService {

    //注入引用数据用Autowired
    //@Autowired
    //@Qualifier("annoFirstDao") //按照id值 从容器中进行匹配
    //不加 @Qualifier 会按照数据类型从Spring容器中进行匹配  类型为1个时可以省略
    @Resource(name = "annoFirstDao")
    //@Resource 相当于 @Autowired + @Qualifier
    private AnnoFirstDao annoFirstDao;

    //注入普通数据用@Value  一般和el表达式一起用才有价值
    @Value("${jdbc.driverClass}")
    private String driver;

    //用注解开发 不用 set方法也行
    /*public void setAnnoFirstDao(AnnoFirstDao annoFirstDao) {
        this.annoFirstDao = annoFirstDao;
    }*/

    @Override
    public void method() {
        System.out.println("-------------------------------");
        System.out.println("driver: " + driver);
        System.out.println("-------------------------------");
        annoFirstDao.method();
    }

}
```



使用@PostConstruct标注初始化方法，使用@PreDestroy标注销毁方法

```java

@PostConstruct
public void init(){
	System.out.println("初始化方法....");
}

@PreDestroy
public void destroy(){
	System.out.println("销毁方法.....");
}

```







####  Spring新注解

​	**使用上面的注解还不能全部替代xml配置文件，还需要使用注解替代的配置如下：**

1. **非自定义的Bean的配置：**``<bean>``
2. **加载properties文件的配置：**``<context:property-placeholder>``
3. **组件扫描的配置：**``<context:component-scan>``
4. **引入其他文件：**``<import>``



<font color="red">**引入Spring新注解**</font>

![新注解](../assets/md-img/SSM.assets/新注解.png)

要想使用注解开发 **首先有一个核心配置类**

``SpringApplicationContext.java``

```java
package com.ganga.config;
/*

* Spring核心配置类
*
* */
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

// 使用@Configuration 定义Spring配置类
@Configuration
// 扫描组件
@ComponentScan("com.ganga")
// 导入其他子配置类
@Import({SpringDataSourceApplicationContext.class})
public class SpringApplicationContext {

}
```



解决 非自定义的Bean的配置 创建DataSource对象

```SpringDataSourceApplicationContext.java```

```java
package com.ganga.config;
import com.mchange.v2.c3p0.ComboPooledDataSource;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.PropertySource;
import javax.sql.DataSource;

//需要引入properties文件
@PropertySource("classpath:jdbc.properties")
public class SpringDataSourceApplicationContext {

    //已经导入了jdbc.properties文件
    //这里直接注入数据
    @Value("${jdbc.driverClass}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.user}")
    private String user;
    @Value("${jdbc.password}")
    private String password;

    //注入自定义的Bean 用@Bean注解 注到方法上 该方法的返回值为bean的对象
    @Bean("dataSourceNew")
    public DataSource getDataSource() throws Exception {
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass(driver);
        dataSource.setJdbcUrl(url);
        dataSource.setUser(user);
        dataSource.setPassword(password);
        return dataSource;
    }

}
```

---







### Spring整合Junit



#### 原始Junit测试Spring的问题

在测试类中，每个测试方法都有以下两行代码：

```java
ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
IAccountService as = ac.getBean("accountService",IAccountService.class);
```

这两行代码的作用是获取容器，如果不写的话，直接会提示空指针异常。所以又不能轻易删掉。

**上述问题解决思路**

	1. 让SpringJunit负责创建Spring容器，但是需要将配置文件的名称告诉它
	1. 将需要进行测试Bean直接在测试类中进行注入



#### Spring集成Junit步骤

1. **导入spring集成Junit的坐标**
1. **使用@Runwith注解替换原来的运行期**
1. **使用@ContextConfiguration指定配置文件或配置类**
1. **使用@Autowired注入需要测试的对象**
1. **创建测试方法进行测试**



<font color="red">**① 导入spring集成Junit的坐标**</font>

```xml
<!--此处需要注意的是，spring5 及以上版本要求 junit 的版本必须是 4.12 及以上-->

<!-- spring-test -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-test</artifactId>
	<version>5.3.18</version>
</dependency>

<!-- junit依赖坐标 -->
<dependency>
	<groupId>junit</groupId>
	<artifactId>junit</artifactId>
	<version>4.13.1</version>
	<!--<scope>test</scope>-->
</dependency>
```



<font color="red">**② 使用@Runwith注解替换原来的运行期**</font>

<font color="red">**③ 使用@ContextConfiguration指定配置文件或配置类**</font>

<font color="red">**④ 使用@Autowired注入需要测试的对象**</font>

<font color="red">**⑤ 创建测试方法进行测试**</font>

`SpringJunitRunTest.java`

```java
package com.ganga.test;

import com.ganga.config.SpringApplicationContext;
import com.ganga.service.UserService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

//第一步: 导入spring-test坐标

//第二步: 使用@Runwith注解替换原来的运行期
@RunWith(SpringJUnit4ClassRunner.class)

//第三步: 指定配置文件/配置类用 @ContextConfiguration
@ContextConfiguration(classes = {SpringApplicationContext.class} )
//@ContextConfiguration("classpath:applicationContext.xml")

public class SpringJunitRunTest {

    //第四步: 使用Autowired注入需要测试的对象
    @Autowired
    private UserService userService;

    //第五步: 创建测试方法进行测试
    @Test
    public void userServiceTest(){
        userService.SelectAll();
    }

    @Autowired
    //@Qualifier("dataSource")
    private DataSource dataSource;
    @Test
    public void dataSourceTest() throws SQLException {
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
        connection.close();
    }

}
```

---









### Spring集成Web环境





#### ApplicationContext应用上下文获取问题

**ApplicationContext应用上下文获取方式**

![image-20220413175942469](../assets/md-img/SSM.assets/image-20220413175942469.png)

**Spring提供了获取应用上下文的工具**

![image-20220413180046845](../assets/md-img/SSM.assets/image-20220413180046845.png)





#### Spring集成Web环境步骤



导入Spring集成Web的坐标

```xml
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-web</artifactId>
	<version>5.3.18</version>
</dependency>
```

在web.xml中配置ContextLoaderListener监听器

```xml
<!-- Spring集成Web环境 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
<listener>
    <!-- 添加C监听器 -->
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<!-- 即: 可以使用WebApplicationContextUtils
         的 getWebApplicationContext() 获取Spring核心容器
 -->
```

使用Spirng-web 提供的工具获取Spring核心容器

```java
//使用Spring-web 提供的工具了
ApplicationContext app = WebApplicationContextUtils.getWebApplicationContext(servletContext);
//要有servletContxt域对象
```



















## SpringMVC框架





### SpringMVC概述

SpringMVC 是一种基于 Java 的**实现 MVC 设计模型的请求驱动类型的轻量级 Web 框架**，属于 SpringFrameWork 的后续产品，已经融合在 Spring Web Flow 中。



SpringMVC 已经成为目前最主流的MVC框架之一，并且随着Spring3.0 的发布，全面超越 Struts2，成为最优秀的 MVC 框架。它**通过一套注解**，让一个简单的 Java 类成为处理请求的控制器，而无须实现任何接口。同时 **它还支持 RESTful 编程风格的请求**。



### SpringMVC快速入门

需求：客户端发起请求，服务器端接收请求，执行逻辑并进行视图跳转。

开发步骤：

1. <font color="red">**导入SpringMVC相关坐标**</font>
2. <font color="red">**配置SpringMVC核心控制器DispathcerServlet**</font>
3. <font color="red">**创建Controller类和视图页面**</font>
4. <font color="red">**使用注解配置Controller类中业务方法的映射地址**</font>
5. <font color="red">**配置SpringMVC核心文件 spring-mvc.xml**</font>
6. <font color="red">**客户端发起请求测试**</font>



<font color="red">**① 导入Spring和SpringMVC的坐标**</font>

<font color="red">**① 导入Servlet和Jsp的坐标**</font>

```xml
<!-- Spring/SpringMVC 框架坐标 -->

<!-- springMVC依赖坐标 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.18</version>
</dependency>
<!-- spring依赖坐标 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.18</version>
</dependency>
<!-- spring-web工具依赖坐标 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.3.18</version>
</dependency>
<!-- spring-test -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.3.18</version>
</dependency>

<!-- service依赖坐标 -->
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
	<version>4.0.1</version>
	<scope>provided</scope>
</dependency>
<dependency>
	<groupId>javax.servlet.jsp</groupId>
	<artifactId>javax.servlet.jsp-api</artifactId>
	<version>2.3.3</version>
</dependency>

<!-- jsp依赖坐标 -->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
	<artifactId>jsp-api</artifactId>
	<version>2.2</version>
	<scope>provided</scope>
</dependency>
```



<font color="red">**② 在web.xml配置SpringMVC的核心控制器**</font>

```xml
<!-- 第二步: 配置SpringMVC核心控制器 DispathcerServlet -->
<servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- 初始化参数配置spring-web.xml文件位置 -->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
    <!-- 设置每次启动时加载该servlet服务 -->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```



<font color="red">**③ 创建Controller和业务方法**</font>

<font color="red">**③ 创建视图页面index.jsp**</font>

<font color="red">**④ 配置注解**</font>

```java
package com.ganga.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

//第三步: 创建Controller类 和 视图页面
//第四步: 使用注解@Controller 将类 添加到容器当中
@Controller
public class UserController {

    //第四步: 使用注解@RequestMapping 添加请求映射
    @RequestMapping("/method")
    public String method(){
        System.out.println("UserController method run ...");
        return "index.jsp";
    }

}
```



<font color="red">**⑤ 创建spring-mvc.xml**</font>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
               ">

    <!-- 第五步: 配置Spring核心文件 spring-mvc.xml -->
    <!-- 扫描controller组件 -->
    <context:component-scan base-package="com.ganga.controller"/>
   
    <!-- 注意: 在web.xml当中 配置此文件的参数路径！ -->

</beans>
```



<font color="red">**⑥ 访问测试地址**</font>





### SpringMVC流程视图



![image-20220413173457520](../assets/md-img/SSM.assets/image-20220413173457520.png)



### SpringMVC的组件解析



#### **SpringMVC的执行流程**

![image-20220413173712264](../assets/md-img/SSM.assets/image-20220413173712264.png)



① 用户发送请求至**前端控制器DispatcherServlet。 **

② DispatcherServlet收到请求调用HandlerMapping处理器映射器。 

③ 处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果 有则生成)一并返回给DispatcherServlet。

④ DispatcherServlet调用HandlerAdapter处理器适配器。

⑤ HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。

⑥ Controller执行完成返回ModelAndView。

⑦ HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。 

⑧ DispatcherServlet将ModelAndView传给ViewReslover视图解析器。 

⑨ ViewReslover解析后返回具体View。 

⑩ DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。DispatcherServlet响应用户。



#### **SpringMVC 组件解析**



 1. <font color="red">**前端控制器：DispatcherServlet**</font>

    ​	用户请求到达前端控制器，它就**相当于 MVC 模式中的 C**，DispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求，DispatcherServlet 的存在**降低了组件之间的耦合性。**

 2. <font color="red">**处理器映射器：HandlerMapping**</font>

    ​	HandlerMapping 负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

 3. <font color="red">**处理器适配器：HandlerAdapter**</font>

    ​	通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理 器进行执行。

 4. <font color="red">**处理器：Handler处理器：Handler**</font>

    ​	它就是我们开发中要编写的**具体业务控制器**。由 DispatcherServlet 把用户请求转发到 Handler。由 Handler 对具体的用户请求进行处理。

 5. <font color="red">**视图解析器：ViewResolver**</font>

    ​	View Resolver 负责将处理结果生成 View 视图，ViewResolver 首先根据逻辑视图名解析成物理视图名，即 具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。

 6. <font color="red">**视图：View**</font>

    ​	SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView等。**最常用的视图就是 jsp**。一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程 序员根据业务需求开发具体的页面





---

---







#### SpringMVC注解解析

##### <font color="red">**@RequestMapping**</font>

![image-20220413174802168](../assets/md-img/SSM.assets/image-20220413174802168.png)

---

另外:RequestMapping标签里面设置属性: produces = "text/html;charset=UTF-8"

​	设置文件格式 和 字符集编码

​	相当于 response.setContentType("text/html;charset=utf-8");

---



```java
@Controller
/*
这里加了@RequestMapping("/web/x1")注解
    该类里的方法 是"/web/x1"的子项目
*/
@RequestMapping("/web/x1")
public class UserControllerDame02 {
    /*
    * 实际地址:http://localhost:8080/B5_SpringMVC_war/web/x1/dame02
    * */
    @RequestMapping("/dame02")
    public String method(){
        System.out.println("run UserControllerDame02 dame02 ...");
        return "/jsp/dame02.jsp";
        /*
        * 返回字符串前不加 "/"
        *   访问的资源是: http://localhost:8080/B5_SpringMVC_war/web/x1/jsp/dame02.jsp
        *
        * 返回字符串前加上 "/"
        *   访问的资源是: http://localhost:8080/B5_SpringMVC_war/jsp/dame02.jsp
        *
        * [注意!] [所以] 要访问根对应的文件 一定要加上"/"
        *
        * */
    }

}
```

---

```java
package com.ganga.controller.X1_RequestMapping注解;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
//枚举类
import org.springframework.web.bind.annotation.RequestMethod;

//1. 不设置路径

@Controller()
@RequestMapping("web/x1")
public class UserControllerDame03 {

    //value = "" 服务资源路径 value可以省略
    //method = 特定的请求方式 [必须是枚举类型] [ RequestMethod.GET ] [ RequestMethod.POST ]等

    //get请求
    @RequestMapping(value = "/dame03-1",method = RequestMethod.GET)
    public String dame01() {
        System.out.println("run UserControllerDame03 dame03 ...");
        return "/jsp/dame03.jsp";
    }//访问成功

    //post请求
    @RequestMapping(value = "/dame03-2",method = RequestMethod.POST)
    public String dame02() {
        System.out.println("run UserControllerDame03 dame031 ...");
        return "/jsp/dame03.jsp";
    }//访问失败 因为这里用的是get请求 访问不了/web/x1/dame03-2

}

```

---

```java
package com.ganga.controller.X1_RequestMapping注解;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
@RequestMapping("/web/x1")
public class UserControllerDame04 {

    //params = 字符数组   必须携带该字符数组中的所有参数

    @RequestMapping(value = "/dame04-1",params = {"username","id"},method = RequestMethod.GET)
    public String method1(){
        System.out.println("run UserControllerDame04-1 dame04-1 ...");
        return "/jsp/dame04-1.jsp";
    }

    /*
    *   http://localhost:8080/B5_SpringMVC_war/web/x1/dame04
    *   不可以访问
    *
    *   http://localhost:8080/B5_SpringMVC_war/web/x1/dame04?username=xxx
    *   不可以访问
    *
    *   http://localhost:8080/B5_SpringMVC_war/web/x1/dame04?username=xxx&id=xxx
    *   可以访问
    *
    *   必须要带 字符数组中的所有参数
    *
    * */

    //params = {"user!100"} user参数不能说100
    @RequestMapping(value = "/dame04-2",params = {"user!100"},method = RequestMethod.GET)
    public String method2(){
        System.out.println("run UserControllerDame04-2 dame04-2 ...");
        return "/jsp/dame04-2.jsp";
    }

}
```

---

---





##### <font color="red">**组件扫描**</font>

![image-20220413175146615](../assets/md-img/SSM.assets/image-20220413175146615.png)

![组件扫描01](../assets/md-img/SSM.assets/组件扫描01.png)

![组件扫描02](../assets/md-img/SSM.assets/组件扫描02.png)



配置处理器适配器

```xml
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
	<property name="messageConverters">
		<list>
			<bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
			</list>
	</property>
</bean>


<!-- 上面比较繁琐 -->
<!-- 直接 -->
<!-- mvc的注解驱动 -->
<mvc:annotation-driven/>
<!--
	以后每次创建mvc的配置文件spring-mvc.xml时
	都先加上这句话
-->
```







##### <font color="red">**视图解析器**</font>

```xml
<!--配置内部资源视图解析器-->

<!-- 配置视图解析器 ViewResolver -->
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<!-- 配置前缀 -->
	<property name="prefix" value="/jsp/"/>
	<!-- 配置后缀 -->
	<property name="suffix" value=".jsp"/>
</bean>

<!--拼接字符路径
	/jsp/xxx.jsp
	/jsp/xxx/xxx.jsp
-->
```









### SpringMVC的数据响应



#### SpringMVC的数据响应方式

![image-20220413182340971](../assets/md-img/SSM.assets/image-20220413182340971.png)



#### 页面跳转

`forward: 转发`

`redirect: 重定向`

<font color="red">**直接返回字符串**</font>

```java
package com.ganga.controller.X3_SpringMVC数据响应.A页面跳转;

/*
* 返回字符串
*
* 配置ViewResolver视图解析器后
*   不需要写前缀 和 后缀
*
* */

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/web/x3")
public class ReturnStringSkip {

    @RequestMapping("/dame05-1")
    public String method01(){
        System.out.println("run dame05 method01");
        return "dame05";
    }
    /*
    *   <!-- 配置视图解析器 ViewResolver -->
    *   <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    *       <!-- 配置前缀 -->
    *       <property name="prefix" value="/jsp/"/>
    *       <!-- 配置后缀 -->
    *       <property name="suffix" value=".jsp"/>
    *   </bean>
    *
    *   /jsp/ + dame05 + .jsp
    *   = /jsp/dame05.jsp
    * */


    /*
    * 源码:
    *   public static final String REDIRECT_URL_PREFIX = "redirect:";
    *   public static final String FORWARD_URL_PREFIX = "forward:";
    *
    * 不加: 默认forward: 转发
    *          redirect: 重定向
    * */
    //
    @RequestMapping("/dame05-2")
    public String method02(){
        System.out.println("run dame05 method02");
        return "redirect:http://localhost:8080/B5_SpringMVC_war/jsp/dame05.jsp";
    }
    
    //重定向 绝对路径

}
```





<font color="red">**通过ModerAndView**</font>

```java
package com.ganga.controller.X3_SpringMVC数据响应.A页面跳转;

/*
* 返回一个 ModelAndView对象 进行跳转视图
*
*   Model数据模型
*       addObject(key,obj)
*   View 视图页面
*       setViewName(url)
*
* */

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping("/web/x3")
public class ReturnModelAndViewSkip {

    @RequestMapping("/dame06-1")
    public ModelAndView method01(){
        //创建ModelAndView模型
        ModelAndView modelAndView = new ModelAndView();
        //设置Model数据模型
        modelAndView.addObject("modelKey","123");
        //设置View视图页面
        modelAndView.setViewName("dame06");
        //返回此模型视图对象
        return modelAndView;
    }
    /*
    * 在这个视图文件中 用el表达式可以获取模型数据
    * */


    /**
     * Spring可以提供注入 把参数设置为注入的对象
     * @return
     */
    @RequestMapping("/dame06-2")
    public ModelAndView method02(ModelAndView modelAndView){
        /* 这里的modelAndView 是由Spring容器自动提供注入的 特性 */
        //设置Model数据模型
        modelAndView.addObject("modelKey","321");
        //设置View视图页面
        modelAndView.setViewName("dame06");
        //返回此模型视图对象
        return modelAndView;
    }

}
```





<font color="red">**上面两个进行拆分**</font>

```java
package com.ganga.controller.X3_SpringMVC数据响应.A页面跳转;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

/*
 *   ReturnModelAndViewSkip
 *   ReturnStringModelSkip
 *
 *   进行拆分
 * */
@Controller
@RequestMapping("/web/x3")
public class ReturnZStringModelSkip {

    @RequestMapping("/dame06-m")
    public String method(Model model){
        //设置模型数据
        model.addAttribute("modelKey","model...");
        //直接跳转页面字符串
        return "dame06";
    }

}
```





#### 回写数据

Web基础阶段，客户端访问服务器端，如果想直接回写字符串作为响应体返回的话，只需要使用 response.getWriter().print(“hello world”) 即可，那么在Controller中想直接回写字符串该怎样呢？

① 通过SpringMVC框架注入的response对象，使用response.getWriter().print(“hello world”) 回写数 据，此时不需要视图跳转，业务方法返回值为void。

```java
//SpringMVC框架会自动识别并注入的response对象
@RequestMapping("/quick4")
public void quickMethod4(HttpServletResponse response) throws IOException {
	response.getWriter().print("hello world");
}
```

② 将    的字符串直接返回，但此时需要通过**@ResponseBody**注解告知SpringMVC框架，方法 返回的字符串不是跳转是直接在http响应体中返回。

```java
@RequestMapping("/quick5")
@ResponseBody
public String quickMethod5() throws IOException {
return "hello springMVC!!!";
}
```





<font color="red">**直接返回字符串**</font>

在异步项目中，客户端与服务器端往往要进行json格式字符串交互，此时我们可以手动拼接json字符串返回。

上述方式手动拼接json格式字符串的方式很麻烦，开发中往往要将复杂的java对象转换成json格式的字符串， 我们可以使用web阶段学习过的json转换工具jackson进行转换，导入jackson坐标。

```xml
<!--jackson-->
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-core</artifactId>
	<version>2.9.0</version> 
</dependency>

<!-- json -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.0</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.9.0</version>
</dependency>
```

代码演示

```java
package com.ganga.controller.X3_SpringMVC数据响应.B回写数据;

import com.alibaba.fastjson.JSON;
import com.ganga.pojo.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Controller
@RequestMapping("/web/x3")
public class WriterBackAttribute {

    /*
    * 回写数据
    * */

    //过SpringMVC框架注入的response对象
    @RequestMapping("/dame08-1")
    public void me01(HttpServletResponse response) throws IOException {
        response.getWriter().print("response! me01");
    }

    //使用@ResponseBody注解
    //注解告知SpringMVC框架，方法
    //返回的字符串不是跳转是直接在http响应体中返回。
    @RequestMapping("/dame08-2")
    @ResponseBody
    public String me02(){
        /*没有参数 返回的字符串不是跳转的页面 而是回写的内容*/
        return "response! me02";
    }

    //手动回写JSON数据
    @RequestMapping("/dame08-3")
    @ResponseBody
    public String me03(){
        //手动书写JSON {"username":"ganga","age":18}
        return "{\"username\":\"ganga\",\"age\":18}";
    }

    //使用外部JSON转换工具
    @RequestMapping("/dame08-4")
    @ResponseBody
    public String me04(){
        //模拟一个对象
        User user = new User("gangajiang", 9);
        //使用json转换工具进行转换
        String json = JSON.toJSONString(user);
        return json;
    } // {"age":9,"username":"gangajiang"}

    /*
    * 上面太麻烦了
    *     SpringMVC提供了相应的机制！
    *
    * */


    //使用外部JSON转换工具
    @RequestMapping("/dame08-5")
    @ResponseBody
    public User me05(){
        System.out.println("run dame08-5 me05...");
        //模拟一个对象
        User user = new User("obg", 99);

        return user;
    }

}
```







<font color="red">**返回对象或集合**</font>

通过SpringMVC帮助我们对对象或集合进行json字符串的转换并回写，为处理器适配器配置消息转换参数， 指定使用jackson进行对象或集合的转换，因此需要在spring-mvc.xml中进行配置

在方法上添加@ResponseBody就可以返回json格式的字符串，但是这样配置比较麻烦，配置的代码比较多， 因此，我们可以使用mvc的注解驱动代替上述配置。

```xml
<!--配置处理器适配器 配置json工具-->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="messageConverters">
        <list>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
        </list>
    </property>
</bean>
<!-- 上面比较繁琐 -->
<!-- 直接 -->
<!-- mvc的注解驱动 -->
<mvc:annotation-driven/>
<!--
    以后每次创建mvc的配置文件spring-mvc.xml时
        都先加上这句话
-->
```

代码演示

```java
package com.ganga.controller.X3_SpringMVC数据响应.B回写数据;

import com.ganga.pojo.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.ArrayList;
import java.util.List;

@Controller
@RequestMapping("/web/x3")
public class WriterBackAttributeObjAndList {

    /*
    * 回写数据
    *   返回值是个对象 WriterBackAttribute 的me05演示过了
    * */
    @RequestMapping("/dame09-1")
    @ResponseBody
    public User me01() {
        //模拟一个对象
        User user = new User("obg-me01", 99);
        return user;
    }

    /*
    * 返回一个集合
    *
    *
    * */

    @RequestMapping("/dame09-2")
    @ResponseBody
    public List<User> me02() {
        //模拟一个对象集合
        List<User> users = new ArrayList<User>();
        users.add(new User("lisa", 17));
        users.add(new User("zs", 99));
        users.add(new User("ls", 66));
        return users;
    }

}

```







### SpringMVC获取请求数据



#### 获取的数据类型

![image-20220413185144429](../assets/md-img/SSM.assets/image-20220413185144429.png)

##### 1. 基本数据类型

Controller中的业务方法的参数名称要与请求参数的name一致，参数值会自动映射匹配。

```java
package com.ganga.controller.X4_SpringMVC获取请求数据.A普通数据类型;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/web/x4")
public class UserRequestParameterController01 {

    @RequestMapping("/user01")
    @ResponseBody
    public String user01(String username, int age) {
        System.out.println(username + " : " + age);
        String write = "<h1 align=\"center\">getParameter: username=" + username +
                "  &  age=" + age + "</h1>";
        return write;
    }

    /*
    * 接收普通数据类型方法
    *   将方法的参数设置成为 要获取提交参数的类型 名称要一样
    *
    *   http://localhost:8080/B5_SpringMVC_war/web/x4/user01?username=ganga&age=9
    *
    *   user01(String username, int age){//...}
    *
    * */

}

```





##### 2. 获取POJO类型参数

Controller中的业务方法的POJO参数的属性名与请求参数的name一致，参数值会自动映射匹配。

```java
package com.ganga.controller.X4_SpringMVC获取请求数据.BPOJO数据类型;

import com.ganga.pojo.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/web/x4")
public class UserRequestParameterController02 {

    @RequestMapping("/user02")
    @ResponseBody
    public String user02(User user){
        System.out.println(user);
        String write = "<h1 align=\"center\">getParameter: " +user.toString() + "</h1>";
        return write;
    }

    /*
    * User实体类中的属性
    *    请求参数必须是实体类中的 成员变量(且有set方法)
    *    请求参数 ∈ 成员变量(且有set方法)
    *
    * SpringMVC会自动封装 为一个实体类的对象
    *
    * */

}

```





##### 3. 获取数组数据类型

Controller中的业务方法数组名称与请求参数的name一致，参数值会自动映射匹配。

```java
package com.ganga.controller.X4_SpringMVC获取请求数据.C数组数据类型;

import com.ganga.pojo.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.Arrays;
import java.util.List;

@Controller
@RequestMapping("/web/x4")
public class UserRequestParameterController03 {

    @RequestMapping("/user03")
    @ResponseBody
    public String user03(String[] pramName){
        List<String> strings = Arrays.asList(pramName);
        System.out.println(strings);
        return strings.toString();
    }
    //http://localhost:8080/B5_SpringMVC_war/web/x4/user03?pramName=1111111&pramName=2222222

    /*
    * 参数名称是 abc
    *
    *   (String[] abc) 参数字符数组的名字 就是 abc
    * */

}

```





##### 4.获取集合数据类型

```java
package com.ganga.controller.X4_SpringMVC获取请求数据.D集合数据类型;

import com.ganga.pojo.VO;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/web/x4")
public class UserRequestParameterController04 {

    @RequestMapping("/user04")
    @ResponseBody
    public String user04(VO vo){
        System.out.println(vo.toString());
        return vo.toString();
    }

}
```





#### 解决请求乱码问题

当post请求时，数据会出现乱码，我们可以设置一个过滤器来进行编码的过滤。

```xml
<!-- 配置CharacterEncodingFilter监听器 -->
<filter>
	<filter-name>CharacterEncodingFilter</filter-name>
	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	<init-param>
		<param-name>encoding</param-name>
        <!-- 设置字符集格式 -->
		<param-value>UTF-8</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>CharacterEncodingFilter</filter-name>
    <!-- 拦截地址/* -->
	<url-pattern>/*</url-pattern>
</filter-mapping>

```





#### 资源拦截问题

解决方案

![资源拦截](../assets/md-img/SSM.assets/资源拦截.png)

####  参数绑定注解

<font color="red">**@requestParam**</font>

![image-20220414205351261](../assets/md-img/SSM.assets/image-20220414205351261.png)

代码演示:

```java
package com.ganga.controller.X4_SpringMVC获取请求数据.F注解RequestParam;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

/**
 * @RequestParam 注解演示
 */

@Controller
@RequestMapping("/web/x4")
public class TestRequestParamAnnotation {

    @RequestMapping("/param00")
    @ResponseBody
    public String param00(String username){
        //http://localhost:8080/B5_SpringMVC_war/web/x4/param00?username=ganga  请求到参数: ganga
        //http://localhost:8080/B5_SpringMVC_war/web/x4/param00?user=gangale    请求到参数: null
        System.out.println(username);
        if (username == null){
            return "null";
        }
        return username;
    }
    /*
    * 当请求的参数是username可以接收
    *
    * 如果请求的参数是user是 接收失败
    *
    * */

    /*
     * RequestParam有三个参数:
     *
     *   value: 与请求参数相同,映射成方法参数名
     *   required: 此在指定的请求参数是否必须包含, 默认是true, 提交时没有该参数就会报错
     *   defaultValue: 当没有指定这个请求参数时, 则会使用指定的默认值
     *
     * */

    //使用@RequestParam注解进行映射
    @RequestMapping("/param01")
    @ResponseBody
    public String param01(@RequestParam("name") String username){
        //http://localhost:8080/B5_SpringMVC_war/web/x4/param01?username=ganga      请求失败
        //http://localhost:8080/B5_SpringMVC_war/web/x4/param01?name=gangajiang     请求到参数: gangajiang
        System.out.println(username);
        return username;
    }

    @RequestMapping("/param02-1")
    @ResponseBody
    public String param02_1(@RequestParam(value = "name",required = true) String username){
        //http://localhost:8080/B5_SpringMVC_war/web/x4/param02-1?name=ganga    请求成功
        //http://localhost:8080/B5_SpringMVC_war/web/x4/param02-1               请求失败
        System.out.println(username);
        return username;
    }
    @RequestMapping("/param02-2")
    @ResponseBody
    public String param02_2(@RequestParam(value = "name",required = false) String username){
        //http://localhost:8080/B5_SpringMVC_war/web/x4/param02-2?name=ganga    请求成功
        //http://localhost:8080/B5_SpringMVC_war/web/x4/param02-2               请求成功
        System.out.println(username);
        return username;
    }

    @RequestMapping("/param03")
    @ResponseBody
    public String param03(@RequestParam(value = "name",defaultValue = "default") String username){
        //http://localhost:8080/B5_SpringMVC_war/web/x4/param03?name=ganga   请求到参数: ganga
        //http://localhost:8080/B5_SpringMVC_war/web/x4/param03              请求到参数: default
        System.out.println(username);
        return username;
    }

}
```



---





#### 获取restful参数风格



![image-20220414205741868](../assets/md-img/SSM.assets/image-20220414205741868.png)

![image-20220414205845074](../assets/md-img/SSM.assets/image-20220414205845074.png)



**代码演示:**

```java
package com.ganga.controller.X4_SpringMVC获取请求数据.G获取Restful风格参数;

/*
*
* Restful是一种软件架构风格、设计风格，而不是标准，
* 只是提供了一组设计原则和约束条件。
* 主要用于客户端和服务器交互类的软件，
* 基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存机制等。
*
*
* GET:      用于获取资源
* POST:     用于新建资源
* PUT:      用于更新资源
* DELETE:   用于删除资源
* */

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/web/x4")
public class TestPathVariable {

    @RequestMapping("/restful/{user}")
    @ResponseBody
    public String pathVariable(@PathVariable(value = "user",required = false) String username){
        //http://localhost:8080/B5_SpringMVC_war/web/x4/restful/gangajiang
        System.out.println(username);
        return username;
    }

    //restful风格参数   post: 用于新建资源   可以在映射服务时  添加指定方式 method=RequestMethod.POST
    @RequestMapping(value = "/userAdd/{user}",method = RequestMethod.POST)
    @ResponseBody
    public String pathVariablePost(@PathVariable("user") String username){
        System.out.println(username);
        return username;
    }

}

```

---







#### 自定义转换器



- SpringMVC 默认已经提供了一些常用的类型转换器，例如客户端提交的字符串转换成int型进行参数设置。 
- 但是不是所有的数据类型都提供了转换器，没有提供的就需要自定义转换器，例如：日期类型的数据就需要自 定义转换器。

**开发步骤：**

<font color="red">**① 定义转换器类实现Converter接口 **</font>

<font color="red">**② 在配置文件中声明转换器**</font>

<font color="red">**③ 在中引用转换器**</font>





<font color="red">**① 定义转换器类实现Converter接口 **</font>

```java
package com.ganga.converter;

import org.springframework.core.convert.converter.Converter;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;


/**
 * 自定义 日期时间转换器
 *
 * 实现Converter
 *      泛型有两个
 *      <e1,e2>
 *          e1是接收的参数类型
 *          e2是要转换的类型
 *
 *  重写converter方法
 *
 */
public class DataConverter implements Converter<String, Date> {
    @Override
    public Date convert(String source) {

        SimpleDateFormat simple = new SimpleDateFormat("yyyy-MM-dd:HH-mm-ss");
        Date date = null;
        try {
            date = simple.parse(source);
        } catch (ParseException e) {
            e.printStackTrace();
        }

        return date;
    }
}

```



<font color="red">**② 在配置文件中声明转换器**</font>

`spring-mvc.xml`

```xml

<!-- 声明自定义转换器 -->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <list>
            <!-- 注入自定义时间日期转换类 -->
            <bean class="com.ganga.converter.DataConverter"></bean>
       </list>
    </property>
</bean>
<!-- [注意！！！]
    <mvc:annotation-driven conversion-service="conversionService"/>
    在mvc 注解驱动上 配置转换器
-->
<mvc:annotation-driven conversion-service="conversionService"/>

```



<font color="red">**③ 在中引用转换器**</font>

```java
package com.ganga.controller.X4_SpringMVC获取请求数据.H自定义转换器;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.Date;

@Controller
@RequestMapping("/web/x4/")
public class ConverterConversionServiceFactionBean {

    @RequestMapping("/converter")
    @ResponseBody
    public String converterTest(@RequestParam("date") Date date){
        //http://localhost:8080/B5_SpringMVC_war/web/x4/converter?date=2060/11/11
        System.out.println(date);
        String dateStr = date.toString();
        return dateStr;
    }

}
```

---



#### 获取Servlet相关API

SpringMVC支持使用原始ServletAPI对象作为控制器方法的参数进行注入，常用的对象如下：

**方法参数类型是Servlet相关API SpringMVC回自动注入**

    - **HttpServletResponse**
   - **HttpServletResponse**
   - **HttpSession**

```java
@Controller
@RequestMapping("/web/x4")
public class SpringMvcGetServletAPI {

    //http://localhost:8080/B5_SpringMVC_war/web/x4/getAPI
    @RequestMapping("/getAPI")
    @ResponseBody
    public String getServletAPI(HttpServletRequest request, HttpServletResponse response, HttpSession session) {
        return request + "<br>" + response + "<br>" + session;
    }

}
```

---







#### 获得请求头

---

<font color="red">**@RequestHeader**</font>

​	使用@RequestHeader可以获得请求头信息，

​	相当于web阶段学习的request.getHeader(name)

 @RequestHeader注解的属性如下：

- <font color="red">**value：**</font>请求头的名称

- <font color="red">**required：**</font>是否必须携带此请求头

---

<font color="red">**@CookieValue**</font>

​	使用@CookieValue可以获得指定Cookie的值 

@CookieValue注解的属性如下： 

- <font color="red">**value：**</font>指定cookie的名称 

- <font color="red">**required：**</font>是否必须携带此cookie

---

```java
package com.ganga.controller.X4_SpringMVC获取请求数据.J获取请求头;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.CookieValue;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

/*
* 获取请求头参数
*
* */
@Controller
@RequestMapping("/web/x4")
public class SpringMvcGetHeader {


    /*
    * 注解: @RequestHeader
    *   value指定获取的请求头
    * */
    @RequestMapping("/getHeader")
    @ResponseBody
    public String getHeader(@RequestHeader("User-Agent") String user_agent){
        //http://localhost:8080/B5_SpringMVC_war/web/x4/getHeader
        System.out.println(user_agent);
        return user_agent;
    }

    /*
    * 快速获取请求头参数 Cookie 参数键对应的的值
    * 注解: @CookieValue
    *   直接获取请求体
    *   value cookie的键
    *
    * */
    @RequestMapping("/getCookie")
    @ResponseBody
    public String getCookie(@CookieValue("JSESSIONID") String JSESSIONID){
        //http://localhost:8080/B5_SpringMVC_war/web/x4/getCookie
        System.out.println(JSESSIONID);
        return JSESSIONID;
    }


}
```

---







####  文件上传



<font color="red">**文件上传三要素**</font>

- 表单项type=“file” 

- 表单的提交方式是post 

- 表单的enctype属性是多部分表单形式，及enctype=“multipart/form-data”

![image-20220414211734694](../assets/md-img/SSM.assets/image-20220414211734694.png)



<font color="red">**文件上传原理**</font>

- 当form表单修改为多部分表单时，request.getParameter()将失效。 

- enctype=“application/x-www-form-urlencoded”时，form表单的正文内容格式是：
  - <font color="red">**key=value&key=value&key=value **</font>

- 当form表单的enctype取值为Mutilpart/form-data时，请求正文内容就变成多部分形式：

![文件上传原理](../assets/md-img/SSM.assets/文件上传原理.png)

---





<font color="red">**文件上传步骤 **</font>

- <font color="red">**① 导入fileupload和io坐标 **</font>

- <font color="red">**② 配置文件上传解析器 **</font>

- <font color="red">**③ 编写文件上传代码**</font>



<font color="red">**① 导入fileupload和io坐标 **</font>

`pom.xml`

```xml
<!-- 文件上传及io工具依赖坐标 -->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>
```



<font color="red">**② 配置文件上传解析器 **</font>

`spring-mvc.xml`

```xml
<!-- 配置文件上传解析器 -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!-- 上传文件总大小不超过40MB -->
    <property name="maxUploadSize" value="40960000"/>
    <!-- 上传单个问价大小不超过20MB -->
    <property name="maxUploadSizePerFile" value="20480000"/>
    <!-- 默认编码格式是utf-8 -->
    <property name="defaultEncoding" value="utf-8"/>
</bean>
```



<font color="red">**③ 编写文件上传代码**</font>

`java代码`

```java
package com.ganga.controller.X4_SpringMVC获取请求数据.K文件上传;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

import java.io.File;
import java.io.IOException;

@Controller
@RequestMapping("/web/x4")
public class SendFile {

    /**
     *单文件上传
     * @param filename 提交的文件名
     * @param upload   注意: 提交表单input file的 name的名字 两个参数要的参数名一定要一样
     */
    @RequestMapping("/upload01")
    @ResponseBody
    public String upload01(String filename, MultipartFile upload){
        //http://localhost:8080/B5_SpringMVC_war/jsp/upload.jsp
        System.out.println(filename);
        //obj.getOriginalFilename() 获取文件名称
        String originalFilename = upload.getOriginalFilename();
        try {
            //obj.transferTo(File file) 文件对拷 将文件上传到该file上
            upload.transferTo(new File("D:\\MySSM\\A2_Spring_MVC\\B5_SpringMVC\\src\\main\\webapp\\file\\"+originalFilename));
        } catch (IOException e) {
            e.printStackTrace();
        }
        return "......";
    }


    /**
     * 多文件上传
     * @param fileName
     * @param upload1 表单提交文件 name="upload1"
     * @param upload2 表单提交文件 name="upload2"
     * @param upload3 表单提交文件 name="upload3"
     * @return
     */
    @RequestMapping("/upload02-1")
    @ResponseBody
    public String upload02(String fileName,MultipartFile upload1,MultipartFile upload2,MultipartFile upload3){
        //http://localhost:8080/B5_SpringMVC_war/jsp/upload.jsp
        System.out.println(fileName);
        String name1 = upload1.getOriginalFilename();
        String name2 = upload2.getOriginalFilename();
        String name3 = upload3.getOriginalFilename();
        try {
            upload1.transferTo(new File("D:\\MySSM\\A2_Spring_MVC\\B5_SpringMVC\\src\\main\\webapp\\file\\" + name1));
            upload2.transferTo(new File("D:\\MySSM\\A2_Spring_MVC\\B5_SpringMVC\\src\\main\\webapp\\file\\" + name2));
            upload3.transferTo(new File("D:\\MySSM\\A2_Spring_MVC\\B5_SpringMVC\\src\\main\\webapp\\file\\" + name3));
        } catch (IOException e) {
            e.printStackTrace();
        }
        return "......";
    }

    /**
     * 改进多文件上传
     * @param fileName
     * @param upload  提交一个upload数组
     * @return
     */
    @RequestMapping("/upload02-2")
    @ResponseBody
    public String upload03(String fileName,MultipartFile[] upload){
        //http://localhost:8080/B5_SpringMVC_war/jsp/upload.jsp
        System.out.println(fileName);
        for (MultipartFile multipartFile : upload) {
            String originalFilename = multipartFile.getOriginalFilename();
            try {
                multipartFile.transferTo(new File("D:\\MySSM\\A2_Spring_MVC\\B5_SpringMVC\\src\\main\\webapp\\file\\"+originalFilename));
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return "......";
    }

}
```

`表单:`

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>

<html>
<head>
    <title>Title</title>
</head>
<body style="background: bisque">
<br><br><br>
<div align="center">
    <h1>文件上传 - 单文件上传</h1>
    <form method="post" enctype="multipart/form-data" action="${pageContext.request.contextPath}/web/x4/upload01">
        文件名称:<input type="text" name="filename"><br>
        上传文件:<input type="file" name="upload"><br>
        <input type="submit" value="提交上传">
    </form>
</div>


<br><br><br>
<div align="center">
    <h1>文件上传 - 多文件上传 - 02</h1>
    <form method="post" enctype="multipart/form-data" action="${pageContext.request.contextPath}/web/x4/upload02-1">
        文件名称:<input type="text" name="filename"><br>
        上传文件:<input type="file" name="upload1"><br>
        上传文件:<input type="file" name="upload2"><br>
        上传文件:<input type="file" name="upload3"><br>
        <input type="submit" value="提交上传">
    </form>
</div>


<br><br><br>
<div align="center">
    <h1>文件上传 - 多文件上传 - 01</h1>
    <form method="post" enctype="multipart/form-data" action="${pageContext.request.contextPath}/web/x4/upload02-2">
        文件名称:<input type="text" name="filename"><br>
        上传文件:<input type="file" name="upload"><br>
        上传文件:<input type="file" name="upload"><br>
        上传文件:<input type="file" name="upload"><br>
        <input type="submit" value="提交上传">
    </form>
</div>


</body>
</html>

```









### SpringMVC拦截过滤器

---

自定义拦截器很简单，只有如下三步： 

① 创建拦截器类实现HandlerInterceptor接口 

② 配置拦截器 

③ 测试拦截器的拦截效果

---





#### ① 创建拦截器类实现HandlerInterceptor接口 

```java
package com.ganga.interceptor;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
/*
* 创建一个实现类 实现 HandlerInterceptor接口
*
*   实现/覆盖重写 preHandle()
*               postHandle()
*               afterCompletion()
*               三个方法
*
* */
public class MyInterceptor implements HandlerInterceptor {

    /**
     * 在访问目标执行之前执行
     * @param request
     * @param response
     * @param handler
     * @return 返回值是一个boolean值 true:允许访问 false:不能访问
     * @throws Exception
     */
    @Override
    public boolean preHandle(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler)
        throws Exception {
        
        System.out.println("\n run preHandle ...");
        String user = request.getParameter("user");
        if ("ganga".equals(user)){
            return true;
        }else {
            request.getRequestDispatcher("/error.jsp")
                .forward(request,response);
            return false;
        }
        //true： true:允许访问 false:不能访问
    }

    //在访问目标执行之后 返回视图/视图模型之前 执行
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("\n run postHandle ...");
    }

    //在目标访问执行完毕后 返回视图/视图模型之后 执行收尾之后
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("\n run afterCompletion ...");
    }
}

```



#### ② 配置拦截器

```xml
<!-- 配置拦截器 -->
<mvc:interceptors>
    
    <mvc:interceptor>
        <mvc:mapping path="/web/dame01/**"/>
        <bean class="com.ganga.interceptor.MyInterceptor"/>
    </mvc:interceptor>
    
    <!--
    <mvc:interceptor>
        <mvc:mapping path="/web/dame02/**"/>
        <bean class="com.ganga.interceptor.MyInterceptor02"/>
    </mvc:interceptor>
    -->
</mvc:interceptors>
```



#### ③ 测试拦截器的拦截效果

```java
package com.ganga.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping("/web/dame02")
public class DameController02 {

    @RequestMapping("/me02")
    public ModelAndView method02(ModelAndView modelAndView,@RequestParam("user") String username){
        //http://localhost:8080/B6_SpringMVC_Interceptor_war/web/dame02/me02        不可以访问
        //http://localhost:8080/B6_SpringMVC_Interceptor_war/web/dame02/me02?user=gangajiang 不可以访问
        //http://localhost:8080/B6_SpringMVC_Interceptor_war/web/dame02/me02?user=ganga 可以访问
        modelAndView.addObject("params",username);
        modelAndView.setViewName("/index.jsp");
        System.out.println("\n访问执行...");
        return modelAndView;
    }

}
```



---



#### 拦截器方法

![image-20220426142433228](../assets/md-img/SSM.assets/image-20220426142433228.png)



---



#### 注意: 它是一个chain



与servlet的拦截器filter一样 是一个chain [链]

```xml
<mvc:interceptors>    
    
	<!-- 他是一个Interceptor chain -->
    <mvc:interceptor>
        <mvc:mapping path="/web/dame03/**"/>
        <bean class="com.ganga.interceptor.DameChain01"/>
    </mvc:interceptor>
    
    <mvc:interceptor>
        <mvc:mapping path="/web/dame03/**"/>
        <bean class="com.ganga.interceptor.DameChain02"/>
    </mvc:interceptor>
    
    <!--根据<mvc:interceptor>标签写的前后位置 链的前后顺序
        run preHandle 0001 ...
        run preHandle 0002 ...
        访问执行...
        run postHandle 0002 ...
        run postHandle 0001 ...
        run afterCompletion 0002 ...
        run afterCompletion 0001 ...
    -->
</mvc:interceptors>
```



---

---





### SpringMVC异常处理机制





#### 1.异常处理的思路

![image-20220426144349332](../assets/md-img/SSM.assets/image-20220426144349332.png)



---

#### 2.异常处理两种方式

- 使用Spring MVC提供的简单异常处理器<font color=red>**SimpleMappingExceptionResolver**</font>

- 实现Spring的异常处理接口<font color=red>**HandlerExceptionResolver**</font> 自定义自己的异常处理器

---



#### 3. 方式一:

<font color=red>**简单的异常处理器 SimpleMappingExceptionResolver**</font>

SpringMVC已经定义好了该类型转换器，在使用时可以根据项目情况进行相应异常与视图的映射配置

```xml
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <!--默认异常-->
    <property name="defaultErrorView" value="/index.jsp"/>
    <!--手动添加异常-->
    <property name="exceptionMappings">
        <map>
            <!-- 类转换异常 -->
            <entry key="java.lang.ClassCastException" 
                   value="/error.jsp"/>
            <!-- 除以0的异常 -->
            <entry key="java.lang.ArithmeticException" 
                   value="/error.jsp"/>
            <!-- 找不到资源异常 -->
            <entry key="java.io.FileNotFoundException" 
                   value="/error.jsp"/>
            <!-- 空指针异常 -->
            <entry key="java.lang.NullPointerException" 
                   value="/error.jsp"/>
            <!-- 自定义异常 -->
            <entry key="com.ganga.exception.MyException" 
                   value="/error.jsp"/>
        </map>
    </property>
</bean>
```

---

---



#### 4.方式二:

<font color=red>**自定义异常 HandlerExceptionResolver**</font> 

① 创建异常处理器类实现HandlerExceptionResolver 

② 配置异常处理器 

③ 编写异常页面 

④ 测试异常跳转

---



① 创建异常处理器类实现HandlerExceptionResolver 

```java
package com.ganga.exception;

import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.FileNotFoundException;

public class MyHandlerExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        ModelAndView mav = new ModelAndView();
        //判断异常 并写入相应的逻辑
        if (ex instanceof ClassCastException){
            mav.addObject("errors","类转换异常");
        }else if (ex instanceof ArithmeticException){
            mav.addObject("errors","除0异常");
        }else if (ex instanceof FileNotFoundException){
            mav.addObject("errors","空文件路径异常");
        }else if (ex instanceof NullPointerException){
            mav.addObject("errors","空指针异常");
        }else if (ex instanceof MyException){
            mav.addObject("errors","自定义异常");
        }//这里只是演示而已 一般里面写的是相应的逻辑！

        mav.setViewName("/errors.jsp");

        return mav;
    }
}

```



② 配置异常处理器 

```xml
<!-- 配置自定义异常 HandlerExceptionResolver -->
<bean class="com.ganga.exception.MyHandlerExceptionResolver"/>
```



③ 编写异常页面 

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
<title>Title</title>
</head>
<body>
这是一个最终异常的显示页面
</body>
</html>
```



④ 测试异常跳转





---

---





#### 5.知识要点

![image-20220426145610225](../assets/md-img/SSM.assets/image-20220426145610225.png)





---

---

---







## SpringJdbcTemplate



### JdbcTemplate概述

它是spring框架中提供的一个对象，是**对原始繁琐的Jdbc API对象的简单封装**。spring框架为我们提供了很多的操作模板类。例如：操作关系型数据的JdbcTemplate和HibernateTemplate，操作nosql数据库的RedisTemplate，操 作消息队列的JmsTemplate等等。



 ### JdbcTemplate开发步骤



<font color="red">**① 导入spring-jdbc和spring-tx坐标**</font>

<font color="red">**② 创建数据库表和实体**</font>

<font color="purple">**③ 创建JdbcTemplate对象注入DataSource放入容器**</font>

<font color="red">**④ 执行数据库操作**</font>



---

### 演示操作: 

<font color="red">**① 导入spring-jdbc和spring-tx坐标**</font>

`pom.xml`

```xml
<!-- 导入JdbcTemplate相关桌标 -->
<!-- spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.18</version>
</dependency>
<!-- spring-tx -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.3.18</version>
</dependency>
```

---

<font color="red">**② 创建数据库表和实体**</font>

---

<font color="purple">**③ 创建JdbcTemplate对象注入DataSource放入容器**</font>

`jdbc.properties`

```properties
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///webcase?useSSL=false&useServerPrepStmts=true
jdbc.user=root
jdbc.password=ganga
```


`applicationContext.xml`

```xml
<!-- 添加jdbc.properties文件 -->
<context:property-placeholder location="classpath:jdbc.properties"/>
<!-- 设置相应数据源配置 并 放入Spring容器当中 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="${jdbc.driverClass}"/>
    <property name="jdbcUrl" value="${jdbc.url}"/>
    <property name="user" value="${jdbc.user}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>
<!-- 创建jdbc模板bean 并注入数据源 放入Spring容器当中 -->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

---

<font color="red">**④ 执行数据库操作**</font>

```java
package com.ganga.test;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;
import org.testng.annotations.Test;
import javax.sql.DataSource;
import javax.xml.crypto.Data;
import java.beans.PropertyVetoException;

public class JdbcTemplateTest {

    @Test
    public void entryTest01() throws PropertyVetoException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        //要指定相应的数据源 jdbcTemplate.setDataSource
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass("com.mysql.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql:///webcase?useSSL=false");
        dataSource.setUser("root");
        dataSource.setPassword("ganga");
        //设置数据源信息
        jdbcTemplate.setDataSource(dataSource);

        //执行操作
        int row = jdbcTemplate.update("insert into account values(?,?)", "ganga", 100000000);
        System.out.println(row);

        /*int row = jdbcTemplate.update("delete from account where user = ? ", "ganga");
        System.out.println(row);*/
    }

    /*
    *
    * 上面与之前学过的 进行改造
    *
    * */

    @Test
    public void entryTest02(){
        //直接从容器中获取已经注入好数据源的JdbcTemplate对象
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        JdbcTemplate jdbcTemplate = app.getBean(JdbcTemplate.class);

        //执行操作
        int row = jdbcTemplate.update("insert into account values(?,?)","gangajiang","1230000");
        System.out.println(row);

    }

}
```

---

---

---









### JdbcTemplate的常用操作

<font color="red">**增删改查**</font>

`JdbcTemplateCRUDTest.java`

```java
package com.ganga.test;

import com.ganga.pojo.Account;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.List;

//替换原生测试内核
@RunWith(SpringJUnit4ClassRunner.class)
//加载配置文件
@ContextConfiguration("classpath:applicationContext.xml")
public class JdbcTemplateCRUDTest {

    //注入需要的对象
    @Autowired
    private JdbcTemplate jdbcTemplate;

    //测试添加操作
    @Test
    public void updateAddTest(){
        String sql = "insert into account values(?,?)";
        int row = jdbcTemplate.update(sql, "gangale", "1");
        System.out.println(row);
    }

    //测试删除操作
    @Test
    public void updateDeleteTest(){
        String sql = "delete from account where user = ?";
        int row = jdbcTemplate.update(sql, "gangale");
        System.out.println(row);
    }

    //测试修改操作
    @Test
    public void updateUpdateTest(){
        String sql = "update account set money=? where user=?";
        int row = jdbcTemplate.update(sql,"2","gangale");
        System.out.println(row);
    }


    /**
     * 测试 常用的 查询操作 01
     * <T> List<T> query(String sql,RowMapper<T> rowMapper)
     *
     * RowMapper<T>实现类: BeanPropertyRowMapper<T>
     *
     *  BeanPropertyRowMapper<T> T是要封装的类型
     *
     *  BeanPropertyRowMapper<T>(T.Class) 构造参数是要封装的类型的字节码文件
     *
     *  List<T> BeanPropertyRowMapper<T>(T.Class) 返回的数据类型是List<T>
     *
     *  演示
     *  query(sql, new BeanPropertyRowMapper<Account>(Account.class))
     */
    @Test
    public void querySelectTest01(){
        String sql = "select * from account";
        List<Account> query = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Account>(Account.class));
        System.out.println(query);
    }


    /**
     * 测试 常用的 查询操作 02
     *
     * <T> T queryForObject(
     *      <T> List<T> query(String sql, RowMapper<T> rowMapper, @Nullable Object... args)
     *  )方法
     *
     *  RowMapper<T> rowMapper 和上面一样 只是可以设置参数以及阐述占位符
     *
     *  返回值直接是一个 T 而不是List<T>
     *      也就是说直接返回一个对象
     *
     *  演示
     *  query(sql, new BeanPropertyRowMapper<Account>(Account.class),"ganga")
     */
    @Test
    public void querySelectTest02(){

        String sql = "select * from account where user=?";
        Account ganga = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Account>(Account.class), "ganga");
        System.out.println(ganga);

    }

    /**
     * 测试 常用的 查询操作 02
     *
     * <T> T queryForObject(
     *      <T> List<T> queryForList(String sql, Class<T> elementType)
     *  )方法
     *
     *  Class<T> elementType 可以是一些简单的数据类型 如： Long  Double Integer ...
     *
     *  返回值直接是一个 T 而不是List<T>
     *      也就是说直接返回一个对象
     *
     *
     *
     *  演示
     *  queryForList(sql, Long.class)
     */
    @Test
    public void querySelectTest03(){

        String sql = "select count(*) from account";
        List<Long> longs = jdbcTemplate.queryForList(sql, Long.class);
        System.out.println(longs);

    }

}

```









































## SpringAOP





### JDK 动态代理





演示一

步骤：

1. 定义目标类接口
2. 实现目标类接口 重写目标类方法
3. 创建一个代理工厂类 实现 `InvocationHandler` 实现`invoke()`方法
4. 在invoke()方法中实现 ：1. 增强方法   2. 调用目标方法
5. 通过Proxy.newProxyInstance()方法获取动态代理对象





#### UserDao

```java
package com.ganga.servlet;

public interface UserDao {

    int getAge(String name, int age);

}
```



#### UserDaoImpl

```java
package com.ganga.servlet.impl;

import com.ganga.servlet.UserDao;

public class UserDaoImpl implements UserDao {
    @Override
    public int getAge(String name, int age) {
        System.out.println("目标方法执行......");
        System.out.println("代理前的目标结果是:" + age);
        return age;
    }
}
```



#### UserInvocationHandler [重点]

```java
package com.ganga.handler;

import com.ganga.aspect.MyAspect;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class UserInvocationHandler implements InvocationHandler {

    //创建目标类对象
    Object target;


    /**
     *封装成一个工厂方法 直接获取代理对象的方法
     * @param target
     * @return 返回一个代理对象Proxy.newProxyInstance()
     */
    public Object getInstance(Object target) {
        this.target = target;
        return Proxy.newProxyInstance(
                target.getClass().getClassLoader(),
                target.getClass().getInterfaces(),
                this);
        //this: 字节就是那个实现好的 InvocationHandler
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object ageObj;

        //增强方法
        MyAspect myAspect = new MyAspect();
        myAspect.check_permission();

        //目标方法
        ageObj = method.invoke(target, args);

        //增强方法
        if (ageObj != null){
            int age = (int) ageObj;
            age = age + 1;
            ageObj = age;
        }
        myAspect.log();

        //返回目标方法的结果
        return ageObj;
    }
}
```





#### MyAspect

```java
package com.ganga.aspect;
//这是一个切面
public class MyAspect {
    public void check_permission(){
        System.out.println("检查权限...");
    }
    public void log(){
        System.out.println("日志记录...");
    }
}
```



#### DameUser

```java
package com.ganga;

import com.ganga.handler.UserInvocationHandler;
import com.ganga.servlet.UserDao;
import com.ganga.servlet.impl.UserDaoImpl;

public class DameUser {

    public static void main(String[] args) {
        
        //创建目标对象
        UserDao userDao = new UserDaoImpl();
        
        //创建代理工厂对象 调用getInstance()方法 返回代理对象
        UserDao proxy = (UserDao) new UserInvocationHandler().getInstance(userDao);

        //调用方法
        int age = proxy.getAge("尴尬酱", 9);

        //输出代理后返回的结果
        System.out.println("增强方法后的 代理后的 返回结果:"+age);

    }

}
```









### Cglib动态代理





#### UserTarget

```java
package com.ganga.service;

public class UserTarget {

    /**
     * 目标方法
     * @param name
     * @param age
     * @return
     */
    public int getAge(String name,int age){
        System.out.println("目标方法执行......");
        System.out.println("目标方法执行age参数值为: " + age);
        return age;
    }

}
```



#### CglibProxy [重点]

```java
package com.ganga.proxy;

import com.ganga.aspect.MyAspect;
import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

//定义一个代理对象工厂类 实现 MethodInterceptor
public class CglibProxy implements MethodInterceptor {

    //目标对象
    Object target;

    /**
     * 创建代理对象的方法
     * @param target 调用时传入相应的目标对象
     * @return
     */
    
    public Object createProxy(Object target){
        //转入目标对象
        this.target = target;

        //创建Enhancer核心增强代码
        Enhancer enhancer = new Enhancer();
        //设置被代理的对象的类型
        enhancer.setSuperclass(target.getClass());
        //设置如何增强 参数需要一个MethodInterceptor 此类即是
        enhancer.setCallback(this);
        //获取动态代理对象
        Object proxy = enhancer.create();
        return proxy;

    }

    /**
     *
     * @param proxy
     * @param method
     * @param args
     * @param methodProxy
     * @return
     * @throws Throwable
     */
    @Override
    public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        //实现增强方法  调用目标前
        MyAspect myAspect = new MyAspect();
        myAspect.check_permission();

        //实现调用目标方法 --反射
        //方式一: 使用原目标对象调用目标方法
        Object ageObj = null;
        ageObj = method.invoke(target, args);
        //方式二: 使用代理对象调用目标方法
        //Object age = methodProxy.invokeSuper(proxy, args);

        //实现增强方法  调用方法后
        if (ageObj != null){
            int age = (int) ageObj;
            age = age + 1;
            ageObj = age;
        }
        myAspect.log();

        //返回代理方法返回结果 可也是增强后的方法
        return ageObj;
    }
}
```





#### MyAspect

```java
package com.ganga.aspect;
//这是一个切面
public class MyAspect {
    public void check_permission(){
        System.out.println("检查权限...");
    }
    public void log(){
        System.out.println("日志记录...");
    }
}
```



#### CglibProxyTest

```java
package com.ganga.test;

import com.ganga.proxy.CglibProxy;
import com.ganga.service.UserTarget;
import net.sf.cglib.proxy.MethodInterceptor;

public class CglibProxyTest {

    public static void main(String[] args) {

        //创建代理对象
        UserTarget userTarget = new UserTarget();
        //创建代理工厂对象
        CglibProxy im = new CglibProxy();
        //调用工厂对象对象 获取动态代理
        UserTarget proxy = (UserTarget) im.createProxy(userTarget);
        //代理后
        int age = proxy.getAge("尴尬酱", 9);

        System.out.println("代理后age的值是: " + age);

    }

/*

检查权限...
目标方法执行......
目标方法执行age参数值为: 9
日志记录...
代理后age的值是: 10

*/
}
```









### AOP面向切面编程



#### 1. 什么是面向切面编程

**AOP** 为 <font color="red">**A**</font>spect <font color="red">**O**</font>riented <font color="red">**P**</font>rogramming 的缩写，意思为**面向切面编程**，是通过**预编译方式**和**运行期**<font color="red">**动态代理**</font> 实现程序功能的统一维护的一种技术。

<font color="red">**动态代理**</font>

AOP 是 OOP 的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是**函数式编程的一种衍生范型**。**利用AOP可以对业务逻辑的各个部分进行隔离**，从而使得业务逻辑各部分之间的**耦合度降低**，提高程序 的可重用性，同时提高了开发的效率。



#### 2. AOP 的作用及其优势

- 作用：在程序运行期间，在不修改源码的情况下对方法进行功能增强 

- 优势：减少重复代码，提高开发效率，并且便于维护



#### 3. AOP 的底层实现

​		实际上，AOP 的底层是**通过 Spring 提供的的动态代理技术实现的**。在**运行期间**，Spring通过动态代理技术动态 的生成代理对象，代理对象方法执行时进行增强功能的介入，在去调用目标对象的方法，从而完成功能的增强。



#### 4. AOP 的动态代理技术

常用的动态代理技术

- JDK 代理 : 基于接口的动态代理技术 

- cglib 代理：基于父类的动态代理技术

![image-20220420185145813](../assets/md-img/SSM.assets/image-20220420185145813.png)



#### 5. JDK的动态代理

首先要有一个目标接口:

```java
package com.ganga.proxy.jdk;
public interface TargetInterface {
    void sell();
}
```

有一个目标类 实现 这个目标接口

```java
package com.ganga.proxy.jdk;

public class Target implements TargetInterface{
    @Override
    public void sell() {
        System.out.println("目标方法执行。。。");
    }
}
```

要增强的代码

```java
package com.ganga.proxy.jdk;
public class MyAdvice {
    public void before(){
        System.out.println("前置增强...");
    }
    public void after(){
        System.out.println("后置增强...");
    }
}
```

动态代理 并 测试

```java
package com.ganga.proxy.jdk;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class RunMain {

    public static void main(String[] args) {
        //目标对象
        Target target = new Target();
        //增强对象
        MyAdvice myAdvice = new MyAdvice();
        //代理对象  返回类型 用 目标接口接收！
        TargetInterface proxy = (TargetInterface) 
            Proxy.newProxyInstance(
            	//参数一
                target.getClass().getClassLoader(),
            	//参数二
                target.getClass().getInterfaces(),
            	//参数三 实现InvocationHandler接口
                new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        myAdvice.before(); //前置增强
                        Object invoke = method.invoke(target, args);//目标方法
                        myAdvice.after(); //后置增强
                        return invoke;
                    }
                }
            
        );

        //执行代理对象 的 目标方法
        proxy.sell();

    }
}
```

----





#### 6. Cglib的动态代理



需要一个目标类  不需要接口 

```java
package com.ganga.proxy.cglib;

public class Target{

    public void sell() {
        System.out.println("目标方法执行。。。");
    }
}
```

要增强的代码

```java
package com.ganga.proxy.cglib;

public class MyAdvice {

    public void before(){
        System.out.println("前置增强...");
    }

    public void after(){
        System.out.println("后置增强...");
    }

}
```

创建动态代理 并 测试

```java
package com.ganga.proxy.cglib;


import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

public class RunMain {

    public static void main(String[] args) {
        //目标对象
        Target target = new Target();
        //增强对象
        MyAdvice myAdvice = new MyAdvice();

        //创建 增强器 对象
        Enhancer enhancer = new Enhancer();
        //指定增强器 要代理的目标类(代理类的父类) 的字节码类型
        enhancer.setSuperclass(target.getClass());
        
        //设置回调 参数 实现 MethodInterceptor()接口
        enhancer.setCallback(new MethodInterceptor() {
            @Override
            public Object intercept
                (Object o,
                 Method method,
                 Object[] objects,
                 MethodProxy methodProxy) throws Throwable {
                
                
                //前置增强
                myAdvice.before();
                //目标方法
                Object invoke = method.invoke(target, args);
                //后置增强
                myAdvice.after();

                return invoke;
            }
        });

        //获取代理对象
        Target proxy = (Target) enhancer.create();

        //执行代理方法
        proxy.sell();

    }
}
```



#### 7. AOP 相关概念

Spring 的 AOP 实现底层就是对上面的动态代理的代码进行了封装，封装后我们只需要对需要关注的部分进行代码编写，并通过配置的方式完成指定目标的方法增强。

 在正式讲解 AOP 的操作之前，我们必须理解 AOP 的相关术语，常用的术语如下：

- <font color="red">**Target（目标对象）**</font>：代理的目标对象 

- <font color="red">**Proxy （代理）**</font>：一个类被 AOP 织入增强后，就产生一个结果代理类 

- <font color="red">**Joinpoint（连接点）**</font>：所谓连接点是指那些被拦截到的点。在spring中,这些点指的是方法，因为spring只支持方 法类型的连接点 

- <font color="red">**Pointcut（切入点）**</font>：所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义 

- <font color="red">**Advice（通知/ 增强）**</font>：所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知 

- <font color="red">**Aspect（切面）**</font>：是切入点和通知（引介）的结合 

- <font color="red">**Weaving（织入）**</font>：是指把增强应用到目标对象来创建新的代理对象的过程。spring采用动态代理织入，而 AspectJ采用编译期织入和类装载期织入





#### 8. AOP 开发明确的事项

1. **需要编写的内容 **

   - 编写核心业务代码（目标类的目标方法）

   - 编写切面类，切面类中有通知(增强功能方法) 

   - 在配置文件中，配置织入关系，即将哪些通知与哪些连接点进行结合

      

2. **AOP 技术实现的内容 Spring 框架监控切入点方法的执行。**

   一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。 

   

3. **AOP 底层使用哪种代理方式**

   在 spring 中，框架会根据目标类是否实现了接口来决定采用哪种动态代理的方式。

---







### 基于XML的AOP开发



#### 快速入门

① 导入 AOP 相关坐标 

② 创建目标接口和目标类（内部有切点） 

③ 创建切面类（内部有增强方法） 

④ 将目标类和切面类的对象创建权交给 spring 

⑤ 在 applicationContext.xml 中配置织入关系

⑥ 测试代码



---



① 导入 AOP 相关坐标 

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.18</version>
</dependency>
 <!-- 这个也要导 aop织入方式 -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.9.1</version>
</dependency>
```



② 创建目标接口和目标类（内部有切点） 

`目标接口`

```java
package com.ganga.aop;
public interface TargetInterface {
    void sell1();
    int sell2(String name,int age);
    int sell3(String name,int age);
}
```

`目标类`

```java
package com.ganga.aop;

public class Target implements TargetInterface {
    @Override
    public void sell1() {
        System.out.println("目标方法执行。。。");
    }

    @Override
    public int sell2(String name,int age) {
        System.out.println("姓名: " + name);
        System.out.println("年龄: " + age);
        return age;
    }

    @Override
    public int sell3(String name, int age){
        System.out.println("姓名: " + name);
        System.out.println("年龄: " + age);
        int i = 1/0; //异常
        return age;
    }
}
```

③ 创建切面类（内部有增强方法） 

```java
package com.ganga.aop;
import org.aspectj.lang.ProceedingJoinPoint;
public class MyAdvice {

    public void before(){
        System.out.println("前置增强...");
    }
    public void afterReturning(){
        System.out.println("后置增强...");
    }
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("环绕前增强....");
        Object proceed = pjp.proceed();
        System.out.println("环绕后增强....");
        return proceed;
    }
    public void throwing(){
        System.out.println("异常抛出后增强...");
    }
    public void after(){
        System.out.println("最终增强....");
    }
}
```



④ 将目标类和切面类的对象创建权交给 spring 

⑤ 在 applicationContext.xml 中配置织入关系

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
               ">

    <!-- 目标对象 -->
    <bean id="target" class="com.ganga.aop.Target"/>

    <!-- 切面对象 -->
    <bean id="myAdvice" class="com.ganga.aop.MyAdvice"/>

    <!-- 配置织入 -->
    <aop:config>
        <!-- 声明切面 -->
        <aop:aspect ref="myAdvice">
            <!-- 配置切面 ——> 切面Aspect: 通知Advice + 切点pointcut  -->
            <aop:before method="before" pointcut="execution(public void com.ganga.aop.Target.sell1())"/>
            <aop:after-returning method="afterReturning" pointcut="execution(public void com.ganga.aop.Target.sell1())"/>

            <!-- 切点表达式: 任意返回值, com.ganga.aop包下的 Target类 的参数是String和int的sell2方法 -->
            <aop:before method="before" pointcut="execution(* com.ganga.aop.Target.sell2(String,int))"/>
            <!-- 切点表达式: 任意返回值, com.ganga.aop及其以下的包 中的 任意类 的参数是任意的sell3方法 -->
            <aop:before method="before" pointcut="execution(* com.ganga.aop..*.sell3(..))"/>

            <!-- 切点表达式的抽取 -->
            <aop:pointcut id="pointcutSell3" expression="execution(* com.ganga.aop..*.sell3(..))"/>

            <!-- 通知的类型: 环绕通知 -->
            <aop:around method="around" pointcut-ref="pointcutSell3"/>
            <!-- 抛出异常通知 -->
            <aop:after-throwing method="throwing" pointcut-ref="pointcutSell3"/>
            <!-- 最终增强 -->
            <aop:after method="after" pointcut-ref="pointcutSell3"/>


        </aop:aspect>
    </aop:config>

    <!--
        pointcut="execution()"
        切点表达式 绑定切点
        execution([修饰符] 返回值类型 包名.类名.方法名(参数列表))
            - 访问修饰符可以省略
            - 返回值类型、包名、类名、方法名可以使用 * 号 代表任意
            - 包名 与 类名之间一个点 . 代表当前包下的类， 两个点 . 表示 当前包 及其 子包 下的类
            - 参数列表可以使用两个点表示任意个数，任意类型的参数列表

    -->

</beans>
```



⑥ 测试代码

```java
package com.ganga.aop;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class TestAOP {

    /**
     * 特别注意:
     *  如果有目标接口 Target目标对象继承了这个接口
     *  那么这里注入的应该是这个接口对象
     *  TargetInterface 的对象！
     */
    @Autowired
    private TargetInterface target;

    @Test
    public void testTarget(){
        System.out.println("=============");
        target.sell1();
        System.out.println("=============");
    }

    @Test
    public void testPointcut1(){
        int age = target.sell2("尴尬", 9);
        System.out.println(age);
    }

    @Test
    public void testPointcut2() throws InterruptedException {
        int age = target.sell3("尴尬酱",9);
        System.out.println(age);
        /*
        *   前置增强...
        *   环绕前增强....
        *   姓名: 尴尬酱
        *   年龄: 9
        *   最终增强....
        *   异常抛出后增强...
        *
        * */
    }

}
```





#### 切点表达式

![切点表达式](../assets/md-img/SSM.assets/切点表达式.png)







#### 通知的类型

![通知的类型](../assets/md-img/SSM.assets/通知的类型.png)

其中 环绕通知比较特殊:

需要参数: ProceedingJoninPoint  

调用proceed()方法 并返回一个对象 也是目标对象的返回

```java
public Object around(ProceedingJoinPoint pjp) throws Throwable {
    System.out.println("环绕前增强....");
    Object proceed = pjp.proceed();
    System.out.println("环绕后增强....");
    return proceed;//一定要有返回值 这是代表了目标方法的返回
}
```





#### 切点表达式的抽取

![image-20220420191739637](../assets/md-img/SSM.assets/image-20220420191739637.png)







知识要点

-  aop织入的配置

  ```xml
  <aop:config>
  	<aop:aspect ref=“切面类”>
  		<aop:before method=“通知方法名称” pointcut=“切点表达式"/>
  	</aop:aspect>
  </aop:config>
  ```

- 通知的类型：前置通知、后置通知、环绕通知、异常抛出通知、最终通知

- 切点表达式的写法：

  ```xml
  execution( [修饰符] 返回值类型 包名.类名.方法名(参数) )
  ```

  



## Spring的事务控制



#### 1. 编程式事务控制

---

编程式事务控制相关对象

编程式事务控制三大对象 

- PlatformTransactionManager 

- TransactionDefinition 

- TransactionStatus

---



##### <font color="red">**PlatformTransactionManager   [接口]  平台事务管理器 **</font>

[视频](https://www.bilibili.com/video/BV1WZ4y1P7Bp?p=137)

 PlatformTransactionManager 接口是 spring 的事务管理器，它里面提供了我们常用的操作事务的方法。

![image-20220422165234108](../assets/md-img/SSM.assets/image-20220422165234108.png)





##### <font color="red">**TransactionDefinition   事务的定义信息对象**</font>

[视频](https://www.bilibili.com/video/BV1WZ4y1P7Bp?p=138)

TransactionDefinition 是事务的定义信息对象，里面有如下方法：

![image-20220422170906857](../assets/md-img/SSM.assets/image-20220422170906857.png)



<font color="red">**事务隔离级别:**</font>

设置隔离级别，可以解决事务并发产生的问题，如**脏读**、**不可重复读**和**虚读**。

- ISOLATION_DEFAULT 

- ISOLATION_READ_UNCOMMITTED 

- ISOLATION_READ_COMMITTED 

- ISOLATION_REPEATABLE_READ 

- ISOLATION_SERIALIZABLE

---

<font color="red">**事务传播行为:**</font>

- <font color="red">**REQUIRED：如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选择（默认值） **</font>

- <font color="red">**SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务） **</font>

- **MANDATORY**：使用当前的事务，如果当前没有事务，就抛出异常 

-  **REQUERS_NEW**：新建事务，如果当前在事务中，把当前事务挂起。 

- **NOT_SUPPORTED**：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起 
- **NEVER**：以非事务方式运行，如果当前存在事务，抛出异常 

- **NESTED**：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行 REQUIRED 类似的操作

- <font color="red">**超时时间**</font>：默认值是-1，没有超时限制。如果有，以秒为单位进行设置 

- **是否只读**：建议查询时设置为只读

---





##### <font color="red">**TransactionStatus    [接口] 事务的运行状态**</font>

TransactionStatus 接口提供的是事务具体的运行状态，方法介绍如下。

![image-20220422172503839](../assets/md-img/SSM.assets/image-20220422172503839.png)





##### <font color="red">**三者之间的关系**</font>



**1. PlatformTransactionManager** 

​	接口  定义行为  接口的实现根据不同的dao层技术不同的api进行实现的

**2. TransactionDefinition**

  封装事务的参数的

**3. TransactionStatus**

 被动的封装事务的状态信息的



1和2是是要具体设置的 如果用Spring的 声明式事务控制 是需要配置的

而3是 1和2配置后，事务运行状态发生被动的封装事务的状态信息的，是不需要配置的



---

---









#### 2. 声明式事务控制

##### <font color="red">**什么是声明式事务控制：**</font>

Spring 的声明式事务顾名思义就是**采用声明的方式来处理事务**。这里所说的声明，就是指在配置文件中声明 ，用在 Spring 配置文件中声明式的处理事务来代替代码式的处理事务。

##### <font color="red">**声明式事务处理的作用：**</font>

- 事务管理不侵入开发的组件。具体来说，业务逻辑对象就不会意识到正在事务管理之中，事实上也应该如 此，因为事务管理是属于系统层面的服务，而不是业务逻辑的一部分，如果想要改变事务管理策划的话， 也只需要在定义文件中重新配置即可
- 在不需要事务管理的时候，只要在设定文件上修改一下，即可移去事务管理服务，无需改变代码重新编译 ，这样维护起来极其方便



织入:         切面:  **业务方法**--> 切点     +   **事务控制**-->通知/增强

##### <font color="red">**注意：**Spring 声明式事务控制底层就是AOP。</font>





---

---





#### 3. 基于XML的声明式事务控制



##### Dao层

```java
package com.ganga.dao;

public interface TransferDao {
    void out(String user,double money);
    void in(String user,double money);
}
```

```java
package com.ganga.dao.impl;

import com.ganga.dao.TransferDao;
import org.springframework.jdbc.core.JdbcTemplate;

public class TransferDaoImpl implements TransferDao {

    private JdbcTemplate jdbcTemplate;
    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    @Override
    public void out(String user, double money) {
        jdbcTemplate.update("update account set money = money-? where user = ?;",money,user);
    }

    @Override
    public void in(String user, double money) {
        jdbcTemplate.update("update account set money = money+? where user = ?;",money,user);
    }
}
```

##### service层

```java
package com.ganga.service;

public interface TransferService {

    public void transfer(String outMan,String inMan,double money);

}
```

```java
package com.ganga.service.impl;

import com.ganga.dao.TransferDao;
import com.ganga.service.TransferService;

public class TransferServiceImpl implements TransferService {

    private TransferDao transferDao;

    public void setTransferDao(TransferDao transferDao) {
        this.transferDao = transferDao;
    }

    /**
     * 业务逻辑
     * @param outMan
     * @param inMan
     * @param money
     */
    @Override
    public void transfer(String outMan, String inMan, double money) {
        transferDao.out(outMan,money);
        System.out.println("======");
        int i = 1/0; //模拟出现异常
        transferDao.in(inMan,money);
    }
}
```

##### 模拟controller层/服务层

```java
package com.ganga.controller;

import com.ganga.service.TransferService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/*
* 这里只是模拟web服务端
* */
public class TransferController {

    public static void main(String[] args) {

        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        TransferService transfer = app.getBean(TransferService.class);

        transfer.transfer("ganga","gangajiang",1);


    }
    
}
```



##### 声明式事务控制的实现分析：

声明式事务控制明确事项：

- 切点是谁?		—— 业务方法
- 谁是通知?		—— 事务增强
- 配置切面?		—— 事务增强+业务方法



##### spring-tx坐标导入

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.18</version>
</dependency>
<!-- spring-tx -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.3.18</version>
</dependency>
```



##### applicationContext.xml的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
                http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
               ">

    <!-- 引入jdbc.properties文件 -->
    <context:property-placeholder location="classpath:jdbc.properties"/>
    
    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driverClass}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.user}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!-- SpringJdbcTemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- dao层 -->
    <bean id="transferDao" class="com.ganga.dao.impl.TransferDaoImpl">
        <property name="jdbcTemplate" ref="jdbcTemplate"/>
    </bean>

    <!-- service层 -->
    <bean id="transferServlet" class="com.ganga.service.impl.TransferServiceImpl">
        <property name="transferDao" ref="transferDao"/>
    </bean>




<!-------------------------------------------------------->



    <!-- 配置平台事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    
    
    <!-- 配置通知: 事务的增强 -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!-- 配置事务的参数属性 TransactionDefinition-->
        <tx:attributes>
            <!-- 业务层的那些方法 事务的属性要设置成扫描 -->
            <tx:method name="transfer" isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="-1" read-only="false"/>
            <tx:method name="sell" isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="-1" read-only="false"/>
            <tx:method name="update*" isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="-1" read-only="false"/>
            <!--<tx:method name="*" read-only="true"/>-->
        </tx:attributes>
    </tx:advice>

    
    
    <!-- 织入: 业务切点+事务增强 -->
    <aop:config>
        <!-- 配置切面  通知引用事务管理 切点业务层的所有方法 -->
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.ganga.service.impl..*.*(..))"/>
        <!--<aop:aspect ref=""> 事务的织入不用这种方式
            <aop:after method="" pointcut="execution()"/>
        </aop:aspect>-->
    </aop:config>



</beans>
```



---

---







##### &lt;tx:method&gt;的相关属性

---

- **name：切点方法名称 **

- **isolation:  事务的隔离级别 **
  - ISOLATION_DEFAULT 
  - ISOLATION_READ_UNCOMMITTED 

  - ISOLATION_READ_COMMITTED 

  - ISOLATION_REPEATABLE_READ 

  - ISOLATION_SERIALIZABLE

- **propogation：事务的传播行为 **
  - **REQUIRED**：如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选择（默认值
  - **SUPPORTS**：支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务）

  - **MANDATORY**：使用当前的事务，如果当前没有事务，就抛出异常 

  - **REQUERS_NEW**：新建事务，如果当前在事务中，把当前事务挂起。 

  - **NOT_SUPPORTED**：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起 
  - **NEVER**：以非事务方式运行，如果当前存在事务，抛出异常 

  - **NESTED**：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行 REQUIRED 类似的操作

- **timeout：**超时时间  默认值是-1，没有超时限制。如果有，以秒为单位进行设置

- **read-only**：是否只读   建议查询时设置为只读

---

---









##### 基于XML实现要点:

① 引入tx 和 aop命名空间

② 配置平台事务管理器 

```xml
<!-- 配置平台事务管理器 基于Dao层的实现技术 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    	
        <property name="dataSource" ref="dataSource"/>
    
</bean>
```

③ 配置事务增强

```xml
<!-- 配置通知: 事务的增强 -->
<tx:advice id="txAdvice" 
           transaction-manager="transactionManager">
    
    <!-- 配置事务的参数属性 TransactionDefinition-->
    <tx:attributes>
        
        <!-- 业务层的那些方法 事务的属性要设置成扫描 -->
        
        <tx:method name="transfer" 
                   isolation="REPEATABLE_READ" 
                   propagation="REQUIRED" 
                   timeout="-1" read-only="false"/>
        
        <tx:method name="sell" 
                   isolation="REPEATABLE_READ" 
                   propagation="REQUIRED" 
                   timeout="-1" read-only="false"/>
        
    </tx:attributes>
</tx:advice>
```

④ 配置事务 AOP 织入

```xml
<!-- 织入: 业务切点+事务增强 -->
<aop:config>
    <!-- 配置切面  通知引用事务管理 切点业务层的所有方法 -->
    <aop:advisor advice-ref="txAdvice" 
                 pointcut="execution(切点表达式)"/>
</aop:config>
```





---

---

---





#### 4. 基于注解的声明式事务控制



① 使用 **@Transactional **在需要进行事务控制的类或是方法上修饰，注解可用的属性同 xml 配置方式，例如隔离 级别、传播行为等。 

② 注解使用在类上，那么该类下的所有方法都使用同一套注解参数配置。 

③ 使用在方法上，不同的方法可以采用不同的事务参数配置。 

④ Xml配置文件中要开启事务的注解驱动**<tx:annotation-driven />**



---

---





##### 注解: @Transationall

##### Service

```java
package com.ganga.service.impl;

import com.ganga.dao.TransferDao;
import com.ganga.service.TransferService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Isolation;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

@Service
//作用在类上 该类的所有方法都被事务管理
@Transactional(isolation=Isolation.REPEATABLE_READ, propagation = Propagation.REQUIRED, timeout = -1, readOnly = false)
public class TransferServiceImpl implements TransferService {

    //注入Dao对象
    @Autowired
    private TransferDao transferDao;

    /**
     * 业务逻辑
     * @param outMan 转出人
     * @param inMan  转入人
     * @param money  转账金额
     */
    @Override
    //作用在方法上 该方法被该事务管理   这里使用该方法的事务
    @Transactional(isolation=Isolation.REPEATABLE_READ, propagation = Propagation.REQUIRED, timeout = -1, readOnly = false)
    public void transfer(String outMan, String inMan, double money) {
        transferDao.out(outMan,money);
        System.out.println("======");
        int i = 1/0;
        transferDao.in(inMan,money);
    }
}
```



---

---





##### 事务注解驱动

##### applicationContext.xml

```xml
<!-- 创建平台事务管理器 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
<!-- 事务管理驱动 -->
<tx:annotation-driven transaction-manager="transactionManager"/>
```













---

---

---











## MyBatis框架 -- 复习



###  MyBatis初始化配置

---

![image-20220316171140457](../assets/md-img/SSM.assets/image-20220316171140457.png)



#### MyBatis官网文档地址：

[https://mybatis.org/mybatis-3/zh/getting-started.html](https://mybatis.org/mybatis-3/zh/getting-started.html)





#### 目录结构:

![image-20220316174352763](../assets/md-img/SSM.assets/image-20220316174352763.png)

注意: 这个是没有使用Mapper代理开发的目录格式

Mapper代理开发目录格式 // 优化格式 将资源配置文件放在 resources当中 但是打包后又刚好和java class文件在同一目录下

![image-20220316194558651](../assets/md-img/SSM.assets/image-20220316194558651.png)









#### pom.xml

**Maven -> pom.xml核心配置文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>B5_MyBatis</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>16</maven.compiler.source>
        <maven.compiler.target>16</maven.compiler.target>
    </properties>

    <!--导入依赖坐标-->
    <dependencies>
        <!--MyBatis 依赖-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.6</version>
        </dependency>
        <!--MySQL 驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.46</version>
        </dependency>
        <!--junit 单元测试-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.1</version>
            <scope>test</scope>
        </dependency>
        <!--  添加slf4j日志api  -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.20</version>
        </dependency>
        <!--  添加logback-classic依赖  -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>
        <!--  添加logback-core依赖  -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
            <version>1.2.3</version>
        </dependency>

    </dependencies>


    <!--
    Maven由于它的[约束] 大于 [配置], 有些配置文件会出现无法被导出或者生效的问题
    解决方案: 在build中配置resources, 来防止我们的资源导出失败问题。
    -->
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>

            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>

</project>


```





#### mybatis-config.xml



**MyBatis -> mybatis-config.xml核心配置文件**

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!--数据库连接信息-->
            <dataSource type="POOLED">
                <!--驱动目录-->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <!--连接地址-->
                <property name="url" value="jdbc:mysql:///数据库名?useSSL=false&amp;useUnicode=true&amp;characterUnicode=utf-8C"/>
                <!--数据库用户名-->
                <property name="username" value=""/>
                <!--数据库密码-->
                <property name="password" value=""/>
            </dataSource>
        </environment>
    </environments>
	
    <!--加载sql映射文件-->
    <mappers>
        <!--加载sql映射文件-->
        <!-- <mapper resource="com/ganga/mapper/UserMapper.xml"/> -->

        <!--Mapper代理方式-->
        <!--扫描该包下的所有映射文件-->
        <package name="com.ganga.mapper"/>
    </mappers>

</configuration>

```





#### XxxMapper.xml模板一

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace: 名称空间 Mapper代理开发 要绑定接口-->
<mapper namespace="com.">
    <!--
    由于java使用的是驼峰命名发 而 sql用的是 _ 命名 有映射问题
    无上述映射问题时 使用:
	
	id名要和映射的接口的抽象方法对象
	resultType与pojo对应的封装类对应 全包名
    -->

    <select id="" resultType="">
        select * from user;
    </select>

    <!--
        参数占位符:
            1. #{}: 会将其代替为 ?, 为了防止SQL注入
            2. ${}: 拼sql, 会存在SQL注入问题
            使用时机:
                1. 参数传递的时候: #{}
                2. 一般不用${} 表名或列名不固定的情况下可以使用${}
                      但是依然会存在SQL注入问题！
        特殊字符处理:
            1. 转义字符
            2. CDATA区:
                    <![CDATA[
                        符号
                    ]]>
                    
             常用转义字符
            "	    	 &quot;      &#34;		    
            &		     &amp;       &#38;		    
            <		     &lt;        &#60;		    
            >		     &gt;        &#62;		    
            不断开空格     &nbsp;      &#160;    	
    -->

</mapper>
```





#### XxxMapper.xml模板二

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace: 名称空间 Mapper代理开发 要绑定接口-->
<mapper namespace="com.ganga.mapper.BrandMapper">
    <!--
    由于java使用的是驼峰命名发 而 sql用的是 _ 命名
    会发生映射不上问题
    可以配置BrandMapper.xml文件 来解决映射问题
    解决方案：
        1. 起别名 [补退选]
        2. resultMap
    
    resultMap标签中的两个属性: 
        id: 唯一标识
        type: 映射的类型,绑定的pojo, 支持别名
        
    resultMap标签中的两个标签:
        id标签: 完成主键字段的映射
        result标签: 完成一般字段的映射
        
    -->
    
    <resultMap id="brandResultMap" type="com"> <!--pojo-->
        
        <result column="数据库" property="java类变量"/>
        <!--column:数据库的列名  property映射类变量名-->
    </resultMap>

    <!--
        把resultType改成resultMap
        给定resultMap的id唯一标识即可完成映射

		id名要和映射的接口的抽象方法对象
		resultType与pojo对应的封装类对应 全包名
    -->
    <select id="" resultMap="brandResultMap">
        select * from tb_brand;
    </select>

    
    
    <!--
        参数占位符:
            1. #{}: 会将其代替为 ?, 为了防止SQL注入
            2. ${}: 拼sql, 会存在SQL注入问题
            使用时机:
                1. 参数传递的时候: #{}
                2. 一般不用${} 表名或列名不固定的情况下可以使用${}
                      但是依然会存在SQL注入问题！
        特殊字符处理:
            1. 转义字符
            2. CDATA区
    -->
    <select id="" resultMap="resultMapId">
        select * from tb_brand where id = #{id};
    </select>
    
</mapper>
```







#### 封装获取SqlSessionFactory对象

```java
package com.ganga.util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//自定义封装工具类
//用于获取MyBatis的 SqlSessionFactory对象从而获取SqlSession对象。
public class MyBatisUtils {

    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            //使用MyBatis第一步: 获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /*
    * 通过该方法获取 sqlSessionFactory对象
    *  
    * sqlSession对象不要进行封装！ 多个资源公用一个sqlSession对象
    *   事务可能会出现问题！
    * */
    public static SqlSessionFactory getSqlSessionFactory(){
        return sqlSessionFactory;
    }


}
```



#### 相关API

#### 执行方式

![执行方式](../assets/md-img/SSM.assets/执行方式.png)





#### 全限定名

![全限定名](../assets/md-img/SSM.assets/全限定名.png)







#### 三个核心类

![三个核心类](../assets/md-img/SSM.assets/三个核心类.png)







### Mapper代理

![image-20220316193821213](../assets/md-img/SSM.assets/image-20220316193821213.png)





### resultMap 映射

```xml
<!--
由于java使用的是驼峰命名发 而 sql用的是 _ 命名
会发生映射不上问题
可以配置BrandMapper.xml文件 来解决映射问题
解决方案：
    1. 起别名 [补退选]
    2. resultMap

resultMap标签中的两个属性:
    id: 唯一标识
    type: 映射的类型, 支持别名

resultMap标签中的两个标签:
    id标签: 完成主键字段的映射
    result标签: 完成一般字段的映射

-->

<resultMap id="brandResultMap" type="com.ganga.pojo.Brand">
    <result column="brand_name" property="brandName"/>
    <result column="company_name" property="companyName"/>
    <!--column:数据库的列名  property映射类变量名-->
</resultMap>


<!--
    把resultType改成resultMap
    给定resultMap的id唯一标识即可完成映射
-->
<select id="selectAll" resultMap="brandResultMap">
    select *
    from tb_brand;
</select>

```







### 参数封装

Mybatis 接口方法中可以接收各种各样的参数，如下：

* 多个参数
* 单个参数：单个参数又可以是如下类型
  * POJO 类型
  * Map 集合类型
  * Collection 集合类型
  * List 集合类型
  * Array 类型
  * 其他类型



常用的三种: 参数接收方式
1. 散装参数: 如果方法中有多个参数, 需要使用( @Param("SQL参数占位符名称")数据类型 参数名称一般和占位符名称相同 )
2. 对象参数: 对象的属性名称要和参数占位符名称一致
3. map集合参数: Map map = new HashMap(); map.put("占位符名称",参数);

#### 1.散装参数: 

```java
/*
默认时Map集合
	以 arg 开头  ：第一个参数就叫 arg0，第二个参数就叫 arg1，以此类推。如：
		map.put("arg0"，参数值1);
		map.put("arg1"，参数值2);
	以 param 开头 ： 第一个参数就叫 param1，第二个参数就叫 param2，依次类推。如：
		map.put("param1"，参数值1);
		map.put("param2"，参数值2);
		
@Param 注解后--------------------------------------
		map.put("自定义key1"，参数值1);
		map.put("自定义key2"，参数值2);
		map.put("param1"，参数值1);
		map.put("param2"，参数值2);
*/

List<Brand> selectByCondition1(@Param("status") int status, @Param("companyName") String companyName, @Param("brandName") String brandName);

	//用户查询的数据
    int status = 1;
    String companyName = "华为";
    String brandName = "华为";
    //数据处理
    companyName = "%" + companyName + "%";
    brandName = "%" + brandName + "%";
    //获取SqlSession对象
    SqlSession sqlSession = MyBatisUtils.getSqlSessionFactory().openSession();
    //获取Mapper代理对象
    BrandMapper brandMapper = sqlSession.getMapper(BrandMapper.class);
    //执行并封装
    List<Brand> brands = brandMapper.selectByCondition1(status, companyName, brandName);
    //释放资源
    sqlSession.close();

```

#### 2.对象参数: 

```java
List<Brand> selectByCondition2(Brand brand);

    //用户查询的数据
    int status = 1;
    String companyName = "华为";
    String brandName = "华为";
    //数据处理
    companyName = "%" + companyName + "%";
    brandName = "%" + brandName + "%";
    //封装参数对象
    Brand brand = new Brand();
    brand.setStatus(status);
    brand.setCompanyName(companyName);
    brand.setBrandName(brandName);
    //获取SqlSession对象
    SqlSession sqlSession = MyBatisUtils.getSqlSessionFactory().openSession();
    //获取Mapper代理对象
    BrandMapper brandMapper = sqlSession.getMapper(BrandMapper.class);
    //执行并封装
    List<Brand> brands = brandMapper.selectByCondition2(brand);
    //释放资源
    sqlSession.close();

```

#### map集合参数:

```JAVA
List<Brand> selectByCondition3(Map map);

    //用户查询的数据
    int status = 1;
    String companyName = "华为";
    String brandName = "华为";
    //数据处理
    companyName = "%" + companyName + "%";
    brandName = "%" + brandName + "%";
    //封装为Map集合
    Map map = new HashMap();
    map.put("status",status);
    map.put("companyName",companyName);
    map.put("brandName",brandName);

    //获取SqlSession对象
    SqlSession sqlSession = MyBatisUtils.getSqlSessionFactory().openSession();
    //获取Mapper代理对象
    BrandMapper brandMapper = sqlSession.getMapper(BrandMapper.class);
    //执行并封装
    List<Brand> brands = brandMapper.selectByCondition3(map);
    //释放资源
    sqlSession.close();
```







#### 提取Sql片段

![image-20220424185412834](../assets/md-img/SSM.assets/image-20220424185412834.png)







### 动态SQL

#### 动态查询

```xml
<!-- 多条件查询 -->
<select id="selectDynamicSQL03" resultMap="brandResultMap">
    select *
    from tb_brand
    <where>
        <if test="status != null">
            status = #{status}
        </if>
        <if test="companyName != null and companyName != ''">
            and company_name like #{companyName}
        </if>
        <if test="brandName != null and brandName != ''">
            and brand_name like #{brandName};
        </if>
    </where>
</select>



<!-------------------------------------------------------------------------->



<!-- 单条件查询 -->
<select id="selectDynamicSQL001" resultMap="brandResultMap">
    select *
    from tb_brand
    where
    <choose> <!--类似于switch-->
        <when test="status != null"> <!--类似于case-->
            status = #{status}
        </when>
        <when test="companyName != null and companyName != ''">
            company_name like #{companyName}
        </when>
        <when test="brandName != null and brandName != ''">
            brand_name like #{brandName};
        </when>
        <otherwise> <!--相当于default-->
            1 = 1
        </otherwise>
    </choose>
</select>
<!--优化上面代码-->
<select id="selectDynamicSQL002" resultMap="brandResultMap">
    select *
    from tb_brand
    <where>
        <choose> <!--类似于switch-->
            <when test="status != null"> <!--类似于case-->
                status = #{status}
            </when>
            <when test="companyName != null and companyName != ''">
                company_name like #{companyName}
            </when>
            <when test="brandName != null and brandName != ''">
                brand_name like #{brandName};
            </when>
        </choose>
    </where>
</select>

```





#### 动态修改

```xml
<!--
	需求: 如果用户只修改其中部分信息
			那么没有修改的会被替换成null
			是非常危险的bug
	解决方案:
			使用: 动态SQL 修改数据
			<set> <if></if> </set> 标签
-->
<update id="updateBrandDynamic">
    update
    tb_brand
    <set>
        <if test="brandName != null and brandName != ''">
            brand_name = #{brandName},
        </if>
        <if test="companyName != null and companyName != ''">
            company_name = #{companyName},
        </if>
        <if test="ordered != null">
            ordered = #{ordered},
        </if>
        <if test="description != null and description != ''">
            description = #{description},
        </if>
        <if test="status != null">
            status = #{status}
        </if>
    </set>
    where
    id = #{id};

</update>
```





#### 动态删除

```xml
<!--
	批量删除
	  问题：不知道用户要删除多少条数据
	          就不知道要使用多少占位符！
	  解决方案：
	      使用动态SQL:
	        <foreach collection="array" item="id" separator="," open="(" close=")">
	            #{id}
	        </foreach>

	其中:
	    collection: 要遍历的集合
	        在MyBatis中会将数组参数, 封装成为一个Map集合
	            默认: array = 数组
	            或使用@Param注解改变Map集合的默认key的名称
	    item: 遍历的每一个结果变量名
	    separator: 没个占位符之间的分隔符 如 "," -> ?,?,?
	    open: 开始时, 前面要加的东西
	    close: 结束时, 后面要加的东西
-->
<delete id="deleteByIdAll">
    delete from tb_brand
    where id
    in  <foreach collection="ids" item="id" separator="," open="(" close=")">
    #{id}
    </foreach>
</delete>

```







### 注解开发

Mybatis 针对 CURD 操作都提供了对应的注解，已经做到见名知意。如下：

* 查询 ：@Select
* 添加 ：@Insert
* 修改 ：@Update
* 删除 ：@Delete



![image-20220318200704169](../assets/md-img/SSM.assets/image-20220318200704169.png)







### MyBatis核心配置文件深入

![image-20220424190404784](../assets/md-img/SSM.assets/image-20220424190404784.png)





#### environments标签

![image-20220424190301714](../assets/md-img/SSM.assets/image-20220424190301714.png)

```xml
<!-- 里面可以设置多个环境 default设置默认环境 "环境名" -->
<environments default="development">
    
    <!-- 环境 id设置环境名称 -->
    <environment id="development">
        <!-- 事务管理器 type设置事务管理器类型  -->
        <transactionManager type="JDBC"/>
        <!--数据库连接信息-->
        <dataSource type="POOLED">
            <!--驱动目录-->
            <property name="driver" value=""/>
            <!--连接地址-->
            <property name="url" value=""/>
            <!--数据库用户名-->
            <property name="username" value=""/>
            <!--数据库密码-->
            <property name="password" value=""/>
        </dataSource>
    </environment>
    
</environments>
```



---





####  mapper标签

该标签的作用是加载映射的，加载方式有如下几种：



使用**相对于类路径的资源引用**，例如：

` <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>`



使用**完全限定资源定位符（URL）**，例如： 

`<mapper url="file:///var/mappers/AuthorMapper.xml"/>`



使用**映射器接口实现类的完全限定类名**，例如： 

`<mapper class="org.mybatis.builder.AuthorMapper"/>`



**将包内的映射器接口实现全部注册为映射器**，例如：

`<package name="org.mybatis.builder"/>`



---





####  Properties标签

实际开发中，习惯将数据源的配置信息单独抽取成一个properties文件，该标签可以加载额外配置的properties文件   --- 使用el表达式

```xml
<!-- 加载properties配置文件 -->
<properties resource="jdbc.properties"></properties>

<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>
        <!--数据库连接信息-->
        <dataSource type="POOLED">
            
            <!-- 这里使用el表达式获取 -->
            <property name="driver" 
                      value="${jdbc.driverClass}"/>
            
            <property name="url" 
                      value="${jdbc.url}"/>
            
            <property name="username" 
                      value="${jdbc.user}"/>
            
            <property name="password" 
                      value="${jdbc.password}"/>
            
        </dataSource>
    </environment>
</environments>
```

```properties
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///webcase?useSSL=false&useServerPrepStmts=true
jdbc.user=root
jdbc.password=ganga
```



----

----



#### typeAliases标签

![image-20220424192553885](../assets/md-img/SSM.assets/image-20220424192553885.png)

---

![image-20220424192658921](../assets/md-img/SSM.assets/image-20220424192658921.png)

















































































---

---

### =========



---

---









### 综合演示





##### mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!--数据库连接信息-->
            <dataSource type="POOLED">
                <!--驱动目录-->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <!--连接地址-->
                <property name="url" value="jdbc:mysql:///webcase?useSSL=false&amp;useUnicode=true&amp;characterUnicode=utf-8C"/>
                <!--数据库用户名-->
                <property name="username" value="root"/>
                <!--数据库密码-->
                <property name="password" value="ganga"/>
            </dataSource>
        </environment>
    </environments>

    <!--加载sql映射文件-->
    <mappers>
        <!--加载sql映射文件-->
        <!-- <mapper resource="com/ganga/mapper/UserMapper.xml"/> -->

        <!--Mapper代理方式-->
        <!--扫描该包下的所有映射文件-->
        <package name="com.ganga.mapper"/>
    </mappers>

</configuration>

```



---



##### MyBatisUtils.java

```java
package com.ganga.util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//自定义封装工具类
//用于获取MyBatis的 SqlSessionFactory对象从而获取SqlSession对象。
public class MyBatisUtils {

    private static SqlSessionFactory sqlSessionFactory;

    static{
        try {
            //使用MyBatis第一步: 获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /*
    * 通过该方法获取 sqlSessionFactory对象
    *
    * sqlSession对象不要进行封装！ 多个资源公用一个sqlSession对象
    *   事务可能会出现问题！
    * */
    public static SqlSessionFactory getSqlSessionFactory(){
        return sqlSessionFactory;
    }

}
```







---



##### UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace: 名称空间 Mapper代理开发 要绑定接口-->
<mapper namespace="com.ganga.mapper.BrandMapper">
    <!--
    由于java使用的是驼峰命名发 而 sql用的是 _ 命名
    会发生映射不上问题
    可以配置BrandMapper.xml文件 来解决映射问题
    解决方案：
        1. 起别名 [补退选]
        2. resultMap

    resultMap标签中的两个属性:
        id: 唯一标识
        type: 映射的类型, 支持别名

    resultMap标签中的两个标签:
        id标签: 完成主键字段的映射
        result标签: 完成一般字段的映射

    -->


    <resultMap id="brandResultMap" type="com.ganga.pojo.Brand">
        <result column="brand_name" property="brandName"/>
        <result column="company_name" property="companyName"/>

        <!--column:数据库的列名  property映射类变量名-->
    </resultMap>


    <!--
        把resultType改成resultMap
        给定resultMap的id唯一标识即可完成映射
    -->
    <select id="selectAll" resultMap="brandResultMap">
        select *
        from tb_brand;

    </select>


    <!--
        参数占位符:
            1. #{}: 会将其代替为 ?, 为了防止SQL注入
            2. ${}: 拼sql, 会存在SQL注入问题
            使用时机:
                1. 参数传递的时候: #{}
                2. 一般不用${} 表名或列名不固定的情况下可以使用${}
                      但是依然会存在SQL注入问题！
    -->

    <select id="selectById" resultMap="brandResultMap">
        select *
        from tb_brand
        where id = #{id};
    </select>


    <!--
        特殊字符处理:
            1. 转义字符
            2. CDATA区:
                    <![CDATA[
                        符号
                    ]]>

             常用转义字符
            "          &quot;      &#34;
            &          &amp;       &#38;
            <          &lt;        &#60;
            >          &gt;        &#62;
            不断开空格     &nbsp;      &#160;
    -->

    <select id="selectByOrdered" resultMap="brandResultMap">
        select *
        from tb_brand
        where ordered &gt;= 50;
    </select>

    <!--
    参数接收方式:
        1. 散装参数: 如果方法中有多个参数, 需要使用( @Param("SQL参数占位符名称")数据类型 参数名称一般和占位符名称相同 )
        2. 对象参数: 对象的属性名称要和参数占位符名称一致
        3. map集合参数: Map map = new HashMap(); map.put("占位符名称",参数);
    -->

    <!--1. 散装参数: 如果方法中有多个参数, 需要使用( @Param("SQL参数占位符名称")数据类型 参数名称一般和占位符名称相同 )-->
    <select id="selectByCondition1" resultMap="brandResultMap">
        select *
        from tb_brand
        where status = #{status}
          and company_name like #{companyName}
          and brand_name like #{brandName};
    </select>
    <!--2. 对象参数: 对象的属性名称要和参数占位符名称一致-->
    <select id="selectByCondition2" resultMap="brandResultMap">
        select *
        from tb_brand
        where status = #{status}
          and company_name like #{companyName}
          and brand_name like #{brandName};
    </select>
    <!--3. map集合参数: Map map = new HashMap(); map.put("占位符名称",参数);-->
    <select id="selectByCondition3" resultMap="brandResultMap">
        select *
        from tb_brand
        where status = #{status}
          and company_name like #{companyName}
          and brand_name like #{brandName};
    </select>


    <!--
    动态获取sql
    -->
    <!--这种查询方式 用户必须要输入全部数据 而不能只输入一个-->
    <select id="selectDynamicSQL01" resultMap="brandResultMap">
        select *
        from tb_brand
        where status = #{status}
          and company_name like #{companyName}
          and brand_name like #{brandName};
    </select>

    <!--动态标签<if>
        依然纯在问题
            如果不查询第一个参数 后面的语句会有一个 and 组成了 where and 不合法
    -->
    <select id="selectDynamicSQL02" resultMap="brandResultMap">
        select *
        from tb_brand
        where
        <if test="status != null">
            status = #{status}
        </if>
        <if test="companyName != null and companyName != ''">
            and company_name like #{companyName}
        </if>
        <if test="brandName != null and brandName != ''">
            and brand_name like #{brandName};
        </if>
    </select>

    <!--
    解决方案:
        使用where标签
        会自动替换 没用的and等
    -->
    <select id="selectDynamicSQL03" resultMap="brandResultMap">
        select *
        from tb_brand
        <where>
            <if test="status != null">
                status = #{status}
            </if>
            <if test="companyName != null and companyName != ''">
                and company_name like #{companyName}
            </if>
            <if test="brandName != null and brandName != ''">
                and brand_name like #{brandName};
            </if>
        </where>

    </select>

<!-- -->
    <!--
    动态获取SQL 查询条件: 单条件动态查询
    -->
    <!--
        需求: 3个公司中选择出一个公司
            再从该公司中模糊查询该商品
    -->
    <select id="selectDynamicSQL001" resultMap="brandResultMap">
        select *
        from tb_brand
        where
        <choose> <!--类似于switch-->
            <when test="status != null"> <!--类似于case-->
                status = #{status}
            </when>
            <when test="companyName != null and companyName != ''">
                company_name like #{companyName}
            </when>
            <when test="brandName != null and brandName != ''">
                brand_name like #{brandName};
            </when>
            <otherwise> <!--相当于default-->
                1 = 1
            </otherwise>
        </choose>
    </select>
    <!--
        使用<otherwise> 1 = 1 </otherwise> 太麻烦
        使用<while>标签改进优化
            <while>标签是非常智能的！
    -->
    <select id="selectDynamicSQL002" resultMap="brandResultMap">
        select *
        from tb_brand
        <where>
            <choose> <!--类似于switch-->
                <when test="status != null"> <!--类似于case-->
                    status = #{status}
                </when>
                <when test="companyName != null and companyName != ''">
                    company_name like #{companyName}
                </when>
                <when test="brandName != null and brandName != ''">
                    brand_name like #{brandName};
                </when>
            </choose>
        </where>
    </select>


    <!--
        添加数据
            获取主键：
            在 insert 标签中添加两个参数
            <insert id="addAll" useGeneratedKeys="true" keyProperty="id">
    -->
    <insert id="addAll" useGeneratedKeys="true" keyProperty="id">
        insert into tb_brand (brand_name, company_name, ordered, description, status)
        values (#{brandName}, #{companyName}, #{ordered}, #{description}, #{status});
    </insert>


    <!--
        修改操作
    -->
    <update id="updateBrand">

        update
            tb_brand
        set brand_name   = #{brandName},
            company_name = #{companyName},
            ordered      = #{ordered},
            description  = #{description},
            status       = #{status}
        where id = #{id};
    </update>

    <!--
        需求: 如果用户只修改其中部分信息
                那么没有修改的会被替换成null
                是非常危险的bug
        解决方案:
        使用: 动态SQL 修改数据
            <set> <if></if> </set> 标签
    -->
    <update id="updateBrandDynamic">
        update
        tb_brand
        <set>
            <if test="brandName != null and brandName != ''">
                brand_name = #{brandName},
            </if>
            <if test="companyName != null and companyName != ''">
                company_name = #{companyName},
            </if>
            <if test="ordered != null">
                ordered = #{ordered},
            </if>
            <if test="description != null and description != ''">
                description = #{description},
            </if>
            <if test="status != null">
                status = #{status}
            </if>
        </set>
        where
            id = #{id};

    </update>


    <!--
        删除操作
    -->
    <delete id="deleteById">
        delete
        from tb_brand
        where id = #{id};
    </delete>


    <!--
    批量删除
      问题：不知道用户要删除多少条数据
              就不知道要使用多少占位符！
      解决方案：
          使用动态SQL:
            <foreach collection="array" item="id" separator="," open="(" close=")">
                #{id}
            </foreach>
    其中:
        collection: 要遍历的集合
            在MyBatis中会将数组参数, 封装成为一个Map集合
                默认: array = 数组
                或使用@Param注解改变Map集合的默认key的名称
        item: 遍历的每一个结果变量名
        separator: 没个占位符之间的分隔符 如 "," -> ?,?,?
        open: 开始时, 前面要加的东西
        close: 结束时, 后面要加的东西
    -->
    <delete id="deleteByIdAll">
        delete from tb_brand
        where id
        in  <foreach collection="ids" item="id" separator="," open="(" close=")">
                #{id}
            </foreach>
    </delete>


</mapper>
```







---





##### pojo - User.java

```java
package com.ganga.pojo;

/*
*
* pojo: tb_brand
*
* */
public class Brand {

    private Integer id;
    private String brandName;
    private String companyName;
    private Integer ordered;
    private String description;
    private Integer status;

    public Brand() {
    }

    public Brand(Integer id, String brandName, String companyName, Integer ordered, String description, Integer status) {
        this.id = id;
        this.brandName = brandName;
        this.companyName = companyName;
        this.ordered = ordered;
        this.description = description;
        this.status = status;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getBrandName() {
        return brandName;
    }

    public void setBrandName(String brandName) {
        this.brandName = brandName;
    }

    public String getCompanyName() {
        return companyName;
    }

    public void setCompanyName(String companyName) {
        this.companyName = companyName;
    }

    public Integer getOrdered() {
        return ordered;
    }

    public void setOrdered(Integer ordered) {
        this.ordered = ordered;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public Integer getStatus() {
        return status;
    }

    public void setStatus(Integer status) {
        this.status = status;
    }

    @Override
    public String toString() {
        return "Brand{" +
                "id=" + id +
                ", brandName='" + brandName + '\'' +
                ", companyName='" + companyName + '\'' +
                ", ordered=" + ordered +
                ", description='" + description + '\'' +
                ", status=" + status +
                '}';
    }
}
```



---



##### Dao层

接口:

```java
package com.ganga.mapper;

import com.ganga.pojo.Brand;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

import java.util.List;
import java.util.Map;

public interface BrandMapper {

    /*
     * 查询所有
     * */
    List<Brand> selectAll();

    /*
     * 通过id查询
     * */
    Brand selectById(int id);

    /*
     *使用到特殊字符
     * 判断ordered>=50的数据
     */
    List<Brand> selectByOrdered();

    /**
     * 条件查询
     *
     * MyBatis 参数封装：
     *
     * 单个参数：
     *      1. POJO类型：直接使用，属性名 和 参数占位符名称 一致
     *      2. Map集合：直接使用，键名 和 参数占位符名称 一致
     *      3. Collection：封装为Map集合，可以使用@Param注解，替换Map集合中默认的arg键名
     *          map.put("arg0",collection集合);
     *          map.put("collection",collection集合);
     *      4. List：封装为Map集合，可以使用@Param注解，替换Map集合中默认的arg键名
     *          map.put("arg0",list集合);
     *          map.put("collection",list集合);
     *          map.put("list",list集合);
     *      5. Array：封装为Map集合，可以使用@Param注解，替换Map集合中默认的arg键名
     *          map.put("arg0",数组);
     *          map.put("array",数组);
     *      6. 其他类型：直接使用
     *
     *
     *   多个参数：封装为Map集合,可以使用@Param注解，替换Map集合中默认的arg键名
     *          	以 arg 开头  ：第一个参数就叫 arg0，第二个参数就叫 arg1，以此类推。如：
     * 	                map.put("arg0"，参数值1);
     * 	                map.put("arg1"，参数值2);
     * 	            以 param 开头 ： 第一个参数就叫 param1，第二个参数就叫 param2，依次类推。如：
     * 	                map.put("param1"，参数值1);
     * 	                map.put("param2"，参数值2);
     *
     *            @Param 注解后--------------------------------------
     * 	            map.put("自定义key1"，参数值1);
     * 	            map.put("自定义key2"，参数值2);
     * 	            map.put("param1"，参数值1);
     * 	            map.put("param2"，参数值2);
     *
     *
     * 常用的三种:
     *      1. 散装参数: 如果方法中有多个参数, 需要使用( @Param("SQL参数占位符名称")数据类型 参数名称一般和占位符名称相同 )
     *      2. 对象参数: 对象的属性名称要和参数占位符名称一致
     *      3. map集合参数: Map map = new HashMap(); map.put("占位符名称",参数);
     *
     * @param status
     * @param companyName
     * @param brandName
     * @return
     */

    /*
    *
    * 默认时Map集合
	*       以 arg 开头  ：第一个参数就叫 arg0，第二个参数就叫 arg1，以此类推。如：
	*	        map.put("arg0"，参数值1);
	*	        map.put("arg1"，参数值2);
	*       以 param 开头 ： 第一个参数就叫 param1，第二个参数就叫 param2，依次类推。如：
	*	        map.put("param1"，参数值1);
	*	        map.put("param2"，参数值2);
	*
    *  @Param 注解后--------------------------------------
	*	    map.put("自定义key1"，参数值1);
	*	    map.put("自定义key2"，参数值2);
	*	    map.put("param1"，参数值1);
	*	    map.put("param2"，参数值2);
    *
    * */
    List<Brand> selectByCondition1(@Param("status") int status, @Param("companyName") String companyName, @Param("brandName") String brandName);

    List<Brand> selectByCondition2(Brand brand);

    List<Brand> selectByCondition3(Map map);


    /**
     * 动态获取SQL
     */
    /*
     * 这种查询方式 用户必须要输入全部数据 而不能只输入一个
     * */
    List<Brand> selectDynamicSQL01(Map map);

    /*
     * 解决方案: 态标签<if>
     *   依然纯在问题:
     *   如果不查询第一个参数 后面的语句会有一个 and 组成了 where and 不合法
     * */
    List<Brand> selectDynamicSQL02(Map map);

    /*
     * 解决方案: 动态标签<where>
     *           会自动替换 没用的and等
     * */
    List<Brand> selectDynamicSQL03(Map map);


    /**
     * 动态获取SQL 查询条件: 单条件动态查询
     */
    /*
     * 需求: 3个公司中选择出一个公司
     *       再从该公司中模糊查询该商品
     *
     *
     * */
    List<Brand> selectDynamicSQL001(Brand brand);

    List<Brand> selectDynamicSQL002(Brand brand);


    /**
     * 添加数据操作
     * 返回值: 是int 受影响的行数
     * <p>
     * 在 insert 标签中添加两个参数
     * <insert id="addAll" useGeneratedKeys="true" keyProperty="id">
     */
    int addAll(Brand brand);


    /**
     * 修改操作:
     */
    int updateBrand(Brand brand);
    /*
    * 动态sql 修改数据
    *   使用 <set> 标签
    * */
    int updateBrandDynamic(Brand brand);



    /**
     *
     * 删除语句
     *
     */
    /* 单个删除 */
    int deleteById(int id);

    /*
    * 批量删除
    *   问题：不知道用户要删除多少条数据
    *           就不知道要使用多少占位符！
    *   解决方案：
    *       使用动态SQL:
    * <foreach collection="array" item="id" separator="," open="(" close=")">
    *       #{id}
    * </foreach>
    *
    * */
    int deleteByIdAll(@Param("ids")int[] ids);


    /**
     *
     *使用注解开发
     *  一些简单的sql语句
     *
     */
    @Select("select * from tb_brand where id = #{id}")
    List<Brand> selectByIdAnnotation(int id);
}

```




---



##### Service层

```java
package com.ganga.service;

import com.ganga.mapper.UserMapper;
import com.ganga.pojo.User;
import com.ganga.util.MyBatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;

import java.util.List;

public class UserService {
    /**
     * 获取工厂
     */
    private SqlSessionFactory factory =
        MyBatisUtils.getSqlSessionFactory();

    /**
     * 登录用户 查询是否存在用户
     *
     * 这里的返回值与Mapper代理方法的返回值不同！
     * @param user
     * @return
     */
    public User selectAll(User user){
        //获取连接对象
        SqlSession sqlSession = factory.openSession();
        //获取代理对象
        UserMapper mapper =
            sqlSession.getMapper(UserMapper.class);
        //执行sql并返回结果
        User us = mapper.selectAll(user);
        //释放资源
        sqlSession.close();

        //判断是否为空
        return us;
    }

    

    /**
     * 查看注册的用户名是否已经存在
     * @param name
     * @return
     */
    public boolean selectByName(String name){
        SqlSession sqlSession = factory.openSession();
        UserMapper mapper = 
            sqlSession.getMapper(UserMapper.class);
        User user = mapper.selectByName(name);

        //释放资源
        sqlSession.close();

        if (user == null){
            //可以注册
            return true;
        }else{
            //不可以注册
            return false;
        }

    }

    /**
     * 注册用户
     * @param username
     * @param password
     * @return
     */
    public int addUser(String username, String password){
        SqlSession sqlSession = factory.openSession();
        UserMapper mapper = 
            sqlSession.getMapper(UserMapper.class);
        int i = mapper.addUser(username, password);

        //提交事务
        sqlSession.commit();
        //释放资源
        sqlSession.close();

        return i;
    }


}
```



---



##### Web层

```java
package com.ganga.web.register;

import com.ganga.service.UserService;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;
import java.io.IOException;

@WebServlet("/register")
public class RegisterServlet extends HttpServlet {
	
    //web层
    private UserService userService = new UserService();

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //注册用户

        //获取用户提交的数据
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        String checkCode = request.getParameter("checkCode");

        System.out.println(username);
        System.out.println(password);
        System.out.println("p5sv".equalsIgnoreCase(checkCode));
        System.out.println(userService.selectByName(username));

        //判断验证码是否正确 p5sv
        if ("p5sv".equalsIgnoreCase(checkCode)){
            //查看注册的用户名是否已经存在
            if (userService.selectByName(username)){
                //可以注册
                userService.addUser(username, password);
                request.setAttribute("register_msg","注册成功, 请登录！");
                request.getRequestDispatcher("/login.jsp").forward(request,response);
            }else {
                //用户名已经存在
                request.setAttribute("register_msg","用户名已经存在！");
                request.getRequestDispatcher("/register.jsp").forward(request,response);
            }
        } else {
            request.setAttribute("register_msg", "验证码错误");
            request.getRequestDispatcher("/register.jsp").forward(request,response);
        }


    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```







































































### 我的踩坑必坑指南



![坑](../assets/md-img/SSM.assets/坑.png)



![image-20220317081542586](../assets/md-img/SSM.assets/image-20220317081542586.png)







### 单词复习

|             单词              |               解释                |
| :---------------------------: | :-------------------------------: |
|      ===================      |        ===================        |
|          environment          |               环境                |
|           resource            |               资源                |
|                               |                                   |
|            mapper             |               映射                |
|             pojo              |              实体类               |
|             utils             |               工具                |
|                               |                                   |
| **@SqlSessionFactoryBuilder** |         SQL会话工厂创建者         |
|                               |                                   |
|    **@SqlSessionFactory**     |            SQL会话工厂            |
|      @. openSession(t/f)      |        获取SqlSession对象         |
|                               |                                   |
|        **@SqlSession**        |              SQL会话              |
|     @ .getMapper(#.class)     |        获取Mapper代理对象         |
|                               |                                   |
|          **Mapper**           |                                   |
|           namespace           |             名称空间              |
|                               |                                   |
|           resultMap           |    结果Map / 数据库与类的映射     |
|             id=""             |        resultMap的唯一标识        |
|            type=""            |      要映射的实体类 全限定名      |
|          <result <id          |  前者完成普通列的映射 后者时主键  |
|           column=""           |            数据库列名             |
|          property=""          |          映射类的属性名           |
|                               |                                   |
|          **@@Param**          | (@Param("status") int 映射参数名) |
|                               |                                   |
|          **dynamic**          |               动态                |
|                               |                                   |
|            <choose            |          选择 -> switch           |
|            <<when             |           何时 -> case            |
|          <<otherwise          |          否则 -> default          |
|                               |                                   |
|           <foreach            |               循环                |
|         collection=""         |       集合 -> 要遍历的集合        |
|            item=""            |         项目 -> 每个结果          |
|         separator=""          |   分离 -> 每个后面要用什么分离    |
|            open=""            |   打开 -> 开始时,前面要加的东西   |
|           close=""            |   关闭 -> 结束时,后面要加的东西   |
|                               |                                   |







## Spring整合MyBatis



### pom.xml

```xml
<packaging>war</packaging>

<dependencies>
    <!-- spring相关 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.18</version>
    </dependency>
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.9.1</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.3.18</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>5.3.18</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.3.18</version>
    </dependency>
    <!-- SpringMVC相关 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.18</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>5.3.18</version>
    </dependency>
    <!-- MyBatis相关 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.6</version>
    </dependency>
    <!-- MyBatis 与 Spring整合 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.3.2</version>
    </dependency>
    <!-- 数据库相关 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.49</version>
    </dependency>
    <!-- 数据源下面任选其一 -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.8</version>
    </dependency>
    <dependency>
        <groupId>c3p0</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.1.2</version>
    </dependency>
    <!-- servlet相关 -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>javax.servlet.jsp-api</artifactId>
        <version>2.3.3</version>
    </dependency>
    <!-- junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.1</version>
    </dependency>
    <!-- JSON -->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.13.2.2</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-annotations</artifactId>
        <version>2.13.2</version>
    </dependency>
    <!-- fastjson -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.76</version>
    </dependency>
    <!-- 文件上传及io工具依赖坐标 -->
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.6</version>
    </dependency>
    <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.4</version>
    </dependency>
    <!-- 日志文件坐标 -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.20</version>
    </dependency>
    <!--  添加logback-classic依赖  -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    <!--  添加logback-core依赖  -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-core</artifactId>
        <version>1.2.3</version>
    </dependency>
</dependencies>
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```



### jdbc.properties

```properties
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///webcase?useSSL=false&useServerPrepStmts=true
jdbc.user=root
jdbc.password=ganga
```



### logback.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--
        CONSOLE ：表示当前的日志信息是可以输出到控制台的。
    -->
    <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>[%level]  %cyan([%thread]) %boldGreen(%logger{15}) - %msg %n</pattern>
        </encoder>
    </appender>

    <logger name="com.ganga" level="DEBUG" additivity="false">
        <appender-ref ref="Console"/>
    </logger>


    <!--

      level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF
     ， 默认debug
      <root>可以包含零个或多个<appender-ref>元素，标识这个输出位置将会被本日志级别控制。
      -->
    <root level="INFO">
        <appender-ref ref="Console"/>
    </root>
</configuration>
```





### mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

    <!-- 引入properties文件 首位！！！ -->
    <!-- 定义别名 -->
    <!-- 配置数据库环境 -->
    <!-- 配置映射器 -->
	
    <!-- 整合后 只需要定义一个别名即可 -->

    <!-- 定义别名 -->
	<typeAliases>
        <!-- 方式一:单个类起别名 -->
        <typeAlias alias="user" type="com.ganga.pojo.User"/>
        <!-- 方式二:扫描改包下的所有类 然后将相应的首字母变小写 -->
        <package name="com.ganga.pojo"/>
        
    </typeAliases>

</configuration>
```



### applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
                http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
               ">

    <!-- 组件扫描 -->
    <!-- 注解驱动 -->
    <!-- 引入properties文件 -->
    <!-- 配置数据源 -->
    
    <!-- Spring整合MyBatis -->
    <!-- 1.配置数据源 -->
    <!-- 2.配置sqlSessionFactory -->
    <!-- 3.1 方式二:配置MapperFactoryBean -->
    <!-- 3.2 方式二:配置MapperScannerConfigurer -->
    
    <!-- 配置声明式事务控制 -->
    <!-- aop织入 -->

    <!-- 组件扫描 -->
    <context:component-scan base-package="com.ganga">
        <!-- 排除的类型是注解！ 不是包 -->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <!-- 注解驱动 -->
    <context:annotation-config></context:annotation-config>

    <!-- 引入properties文件 -->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    

    
    <!-- Spring整合MyBatis -->
    
    <!-- 1. 配置数据源 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driverClass}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.user}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    
    
    <!-- 2. 配置sqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 注入数据源 -->
        <property name="dataSource" ref="dataSource"></property>
        <!-- 加载mybatis核心文件 -->
        <property name="configLocation" value="classpath:mybatis-config.xml"></property>
        <!-- 加载mapper映射文件 -->
        <property name="mapperLocations" value="classpath:com/ganga/mapper/*.xml"/>
    </bean>

    
    <!-- 3.1. 配置方式一: 配置MapperFactoryBean 代理对象 -->
    <!-- userMapper代理对象 -->
    <bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
        <!-- 注入代理接口 -->
        <property name="mapperInterface" value="com.ganga.mapper.UserMapper" />
        <!-- 注入工厂对象 -->
        <property name="sqlSessionFactory" ref="sqlSessionFactory" />
    </bean>
    <!-- accountMapper代理对象 -->
    <bean id="accountMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
        <property name="mapperInterface" value="com.ganga.mapper.AccountMapper"/>
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>
    
    
    <!-- 3.2. 配置方式二: 配置MapperScannerConfigurer 简化MapperFactoryBean -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.ganga.mapper"/>
    </bean>


    
    
    


    <!-- 配置声明式事务控制 基于xml方式-->
    <!-- 事务对象 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!-- 通知/增强 -->
    <tx:advice id="txAdvice">
        <tx:attributes>
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>

    <!-- 织入 -->
    <aop:config>
        <!-- 切面 业务逻辑+事务控制 -->
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.ganga.service.impl..*.*(..))"/>
    </aop:config>

</beans>
```



### spring-mvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
               ">

    <!-- 扫面组件 -->
    <!-- 注解驱动 -->
    <!-- 内部资源视图解析器 -->
    <!-- 开放静态资源访问权限 -->
    <!-- 配置自定义拦截器 -->
    <!-- 配置文件上传解析器 -->
    <!-- 配置异常处理器 -->


    <!-- 扫面组件 -->
    <context:component-scan base-package="com.ganga.controller"/>

    <!-- 注解驱动 -->
    <mvc:annotation-driven conversion-service="conversionService"/>

    <!-- 内部资源视图解析器 -->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/jsp/"/><!-- 前缀 -->
        <property name="suffix" value=".jsp"/><!-- 后缀 -->
    </bean>


    <!-- 开放静态资源访问权限 -->
    <mvc:default-servlet-handler/>


    <!-- 声明自定义日期转换器 -->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <list>
                <!-- 注入自定义时间日期转换类 -->
                <bean class="com.ganga.util.DataConverter"></bean>
            </list>
        </property>
    </bean>
    <!-- [注意！！！]
        <mvc:annotation-driven conversion-service="conversionService"/>
        在mvc 注解驱动上 配置转换器
    -->


    <!-- 配置文件上传解析器 -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!-- 上传文件总大小不超过40MB -->
        <property name="maxUploadSize" value="40960000"/>
        <!-- 上传单个问价大小不超过20MB -->
        <property name="maxUploadSizePerFile" value="20480000"/>
        <!-- 默认编码格式是utf-8 -->
        <property name="defaultEncoding" value="utf-8"/>
    </bean>


    <!-- 配置自定义拦截器 -->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/bean/*"/>
            <bean class="com.ganga.interceptor.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
    
    
    <!-- 配置异常处理器  下面有两种方式: -->
    
    <!-- 配置异常处理器 SimpleMappingExceptionResolver -->
    <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <!--默认异常-->
        <property name="defaultErrorView" value="/index.jsp"/>
        <!--手动添加异常-->
        <property name="exceptionMappings">
            <map>
                <!-- 类转换异常 -->
                <entry key="java.lang.ClassCastException" value="/error.jsp"/>
                <!-- 除以0的异常 -->
                <entry key="java.lang.ArithmeticException" value="/error.jsp"/>
                <!-- 找不到资源异常 -->
                <entry key="java.io.FileNotFoundException" value="/error.jsp"/>
                <!-- 空指针异常 -->
                <entry key="java.lang.NullPointerException" value="/error.jsp"/>
                <!-- 自定义异常 -->
                <entry key="com.ganga.exception.MyException" value="/error.jsp"/>
            </map>
        </property>
    </bean>

    <!-- 配置自定义异常 -->
    <bean class="com.ganga.exception.MyHandlerExceptionResolver"/>

</beans>
```



### web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!-- 监听Spring  监听器-->
    <!-- 监听SpringMVC 配置前端控制器  服务-->
    <!-- 解决中文乱码问题  拦截器-->


    <!-- 监听Spring  监听器-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- 监听SpringMVC 配置前端控制器  服务-->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- 解决中文乱码问题  拦截器-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>


    <!-- 监听器 释放进程 -->
    <listener>
        <listener-class>com.ganga.util.MyServletContextListener</listener-class>
    </listener>

</web-app>
```





### com.xxx.util

#### 日期转换工具

```java
package com.ganga.util;

import org.springframework.core.convert.converter.Converter;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;


/**
 * 自定义 日期时间转换器
 *
 * 实现Converter
 *      泛型有两个
 *      <e1,e2>
 *          e1是接收的参数类型
 *          e2是要转换的类型
 *
 *  重写converter方法
 *
 */
public class DataConverter implements Converter<String, Date> {
    @Override
    public Date convert(String source) {

        SimpleDateFormat simple = new SimpleDateFormat("yyyy-MM-dd:HH-mm-ss");
        Date date = null;
        try {
            date = simple.parse(source);
        } catch (ParseException e) {
            e.printStackTrace();
        }

        return date;
    }
}
```

#### 修复DBCP的BUG

```java
package com.ganga.util;

import com.mysql.jdbc.AbandonedConnectionCleanupThread;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import java.sql.Driver;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Enumeration;

//修复DBCP的BUG，https://issues.apache.org/jira/browse/DBCP-332
public class MyServletContextListener implements ServletContextListener{

    public void contextInitialized(ServletContextEvent servletContextEvent) {

    }

    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        //这里如果Web应用拥有多个数据库的连接，可以一并关闭
        Enumeration<Driver> drivers = DriverManager.getDrivers();
        Driver driver = null;
        while (drivers.hasMoreElements()) {
            try {
                driver = drivers.nextElement();
                DriverManager.deregisterDriver(driver);
            } catch (SQLException ex) {
            }
        }
        AbandonedConnectionCleanupThread.checkedShutdown();
        System.out.println(" 线程释放成功... ");
    }
}
```



#### MyBatisUtils  -- 整合后用不到

```java
package com.ganga.util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//自定义封装工具类
//用于获取MyBatis的 SqlSessionFactory对象从而获取SqlSession对象。
public class MyBatisUtils{

    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            //使用MyBatis第一步: 获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /*
     * 通过该方法获取 sqlSessionFactory对象
     *
     * sqlSession对象不要进行封装！ 多个资源公用一个sqlSession对象
     *   事务可能会出现问题！
     * */
    public static SqlSessionFactory getSqlSessionFactory(){
        return sqlSessionFactory;
    }


}
```







### com.xxx.execption

#### MyHandlerExceptionResolver

```java
package com.ganga.exception;

import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.FileNotFoundException;

public class MyHandlerExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        ModelAndView mav = new ModelAndView();
        //判断异常 并写入相应的逻辑
        if (ex instanceof ClassCastException){
            mav.addObject("errors","类转换异常");
        }else if (ex instanceof ArithmeticException){
            mav.addObject("errors","除0异常");
        }else if (ex instanceof FileNotFoundException){
            mav.addObject("errors","空文件路径异常");
        }else if (ex instanceof NullPointerException){
            mav.addObject("errors","空指针异常");
        }else if (ex instanceof MyException){
            mav.addObject("errors","自定义异常");
        }//这里只是演示而已 一般里面写的是相应的逻辑！

        mav.setViewName("/errors.jsp");

        return mav;
    }
}
```



































