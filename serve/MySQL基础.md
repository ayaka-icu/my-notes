# -数据库的基本概念

## 目录

[TOC]









## 数据库的基本概念

1. 数据库的英文单词： DataBase 简称 ： DB

   

2. 什么数据库？

   * 用于存储和管理数据的仓库。

     

3. 数据库的特点：

   1. 持久化存储数据的。其实数据库就是一个<b><font color='red'>文件系统</font></b>
   2. 方便存储和管理数据
   3. 使用了统一的方式操作数据库 -- SQL

   

4. 常见的数据库软件
   	* Navicat
   	* DataGrip
   	* ......



## 安装&卸载&配置&使用

1. 安装
2. 卸载
   1. 去mysql的安装目录找到my.ini文件		复制 datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"
   2. 卸载MySQL
   3. 删除C:/ProgramData目录下的MySQL文件夹。
3. 配置
   1. MySQL服务启动
      1. 手动。
      2. cmd --> services.msc  打开服务器的窗口
      3. 使用管理员的开cmd( 管理员模式 )
         1. net start mysql80	:	启动mysql的服务
         2. net stop mysql80   :    关闭mysql服务
   2. 登录MySQL
      1. mysql -u用户名 -p密码
      2. mysql -hip -uroot -p连接目标的密码
      3. mysql --host=ip地址 --user=root --password=连接目标的密码
   3. MySQL退出
      1. exit
      2. quit
   4. MySQL目录结构
      1. MySQL安装目录：basedir="安装地址/MySQL/"
         1. 配置文件是 my.ini
      2. MySQL数据目录：basedir="安装地址//MySQL Server 5.5/Data/"
      3. 几个概念：
         1. 数据库：文件夹
         2. 表：文件
         3. 数据：数据



## SQL

1. 什么是SQL？

   Structured Query Language：结构化查询语言

   其实就是定义了操作所有关系型数据库的规则。每一种数据库操作的方式存在不一样的地方，称为“方言”。

   

2. SQL通用语法

   1. SQL 语句可以单行或多行书写，以分号结尾。
   2. 可使用空格和缩进来增强语句的可读性。
   3. MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。
   4. 三种注释：
      1. 单行注释：-- 注释内容  [SQL通用] [注意--后必须加空格]
      2. 单行注释：# 注释内容  [MySQL特有]
      3. 多行注释：/* 注释内容 */

   

3. SQL分类

   ​	![SQL分类](D:\源代码\A2_JavaWeb学习\B1_MySQL学习\src\图解\SQL分类.bmp)

   1. DDL(Data Definition Language)数据定义语言

      ​		用来定义数据库对象：数据库，表，列等。关键字：create, drop,alter 等

   2. DML(Data Manipulation Language)数据操作语言

     ​		用来对数据库中表的数据进行增删改。关键字：insert, delete, update 等

   3. DQL(Data Query Language)数据查询语言

     ​		用来查询数据库中表的记录(数据)。关键字：select, where 等

   4. DCL(Data Control Language)数据控制语言(了解)

     ​		用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT， REVOKE 等
   
   



----

----





## SQL四分类讲解

1. DDL:	操作数据库、表

   ```mysql
   # 用来定义数据库对象：数据库，表，列等。
   # 关键字：create, drop,alter 等
   ```

   

2. DML:	增删改表中的数据

   ```mysql
   # 用来对数据库中表的数据进行增删改。
   # 关键字：insert, delete, update 等
   ```

   

3. DQL：查询表中的记录

   ```mysql
   # 用来查询数据库中表的记录(数据)。
   # 关键字：select, where 等
   ```

   

4. DCL:	数据控制语言 [了解]

   ```mysql
   # 用来定义数据库的访问权限和安全级别，及创建用户。
   # 关键字：GRANT， REVOKE 等
   ```





---

## DDL:	操作数据库、表

操作数据库：CRUD

1. C [ Create ] :   创建
2. R [ Retrieve ] : 查询
3. U [ Updata ] :  修改
4. D [ Delete ] :   删除
5. 使用数据库

----



#### 一：操作数据库

> ---


> ## C [ Create ] :   创建
> 
> 1. 创建数据库：
>    ```mysql
>    create database 数据库名称;
>    ```
> 
> 2. 创建数据库, 判断不纯在, 在创建：
> 
>    ```mysql
>    create database if not exists 数据库名称;
>    ```
> 
> 3. 创建数据库, 并且指定字符集：
> 
>    ```mysql
>    create database 数据库名称 character set 字符集名称;
>    ```
> 
> 4. 综合小练习：创建newgbk数据库, 判断是否存在, 指定gbk编码
> 
>    ```mysql
>    create database if not exists newgbk character gbk;
>    ```
> 
> 
> 
> ## R [ Retrieve ] : 查询
> 
> 1. 查询所有数据库的名称:
> 
>    ```mysql
>    show databases;
>    ```
> 
> 2. 查询某个数据库的字符集:查询某个数据库的创建语
> 
>    ```mysql
>    show create database 数据库名称;
>    ```
> 
> 
> 
> ## U [ Updata ] :  修改
> 
> 1. 修改数据库的字符集
> 
>    ```mysql
>    alter database 数据库名称 character set 字符集名称;
>    ```
> 
> 
> 
> ## D [ Delete ] :   删除
> 
> 1. 删除数据库
> 
>     ```mysql
>     drop database 数据库名称;
>     ```
> 
> 2. 判断数据库存在，存在再删除
> 
>    ```mysql
>    drop database if exists 数据库名称;
>    ```
> 
> 
> 
> ## 使用数据库
> 
> 1. 查询当前正在使用的数据库名称
> 
>    ```mysql
>    select databese();		#要带括号 
>    ```

> 2. 使用数据库
> 
>    ```mysql
>    use 数据库名称;
>    ```
> 
>    
> 
---

---

---



#### 二：操作表

> ---

> ## C [ Create ] :   创建
>
> 1. 语法
>
>    ```mysql
>    create table 表名(
>    	列名1 数据类型1, #用逗号隔开
>        列名2 数据类型2(...),
>        ..... i
>        列名n 数据类型n  #最后一个不带逗号
>    );	#最后别忘了分号结束语句
>    ```
>    
> 2. 数据库类型：
>
>    |   类型    | 类型                                                         |
>    | :-------: | ------------------------------------------------------------ |
>    |    int    | 整数类型                                                     |
>    |  double   | 小数类型                                                     |
>    |  varchar  | 字符串  varchar(字符长度限制 必须要加否则报错)               |
>    |   date    | 日期  包含年月日：yyyy-MM-dd                                 |
>    | datetime  | 日期  包含年月日时分秒：yyyy-MM-dd HH:mm:ss                  |
>    | timestamp | 日期  包含年月日时分秒：yyyy-MM-dd HH:mm:ss <br />特点：如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值 |
>    |  .......  | 还有一些不常见的类型, 一般不会用到                           |
>
> 3. 案例 - 创建一个表
>
>    ```mysql
>    create table 数据类型( #创建一个表,格式: create table 表名称(  ... );
>        字符串 varchar(10), #创建列名称和数据类型：表名称 数据类型 , ...
>        整形 int,     #多个列用 , 隔开
>        小数类型1 double, #double()
>        小数类型2 double(5,2), #设置范围,最大值时99999.99
>        日期1 date,       #日期 只包含年月日 yyyy-MM-dd
>        日期2 datetime,   #日期 包含年月日时分秒,yyyy-MM-dd HH:mm:ss
>        日期3 timestamp,  #日期 获取当前系统日期时间
>        布尔值 boolean,
>        还有一些不常用的 text,
>        end int      #最后一个列不用 , 号，否则会报错
>    );               #最后别忘了括号外面也有一个分号用于结束该整条语句。
>    ```
>
> 4. 另外的：copy到新表
>
>    ```mysql
>    create table 新表名 like 被复制的表名;
>    ```
>
>    
>
> ---
>
> ## R [ Retrieve ] : 查询
>
> 1. 查询某个数据库中所有的表名称
>
>    ```mysql
>    show tables;
>    ```
>
> 2. 查询表结构
>
>    ```mysql
>    desc 表名称;
>    ```
>
> 
>
> ---
>
> ## U [ Updata ] :  修改
>
> 1. 修改表名
>
>    ```mysql
>    alter table 表名 rename to 新表名;
>    ```
>
> 2. 修改表的字符集
>
>    ```mysql
>    alter table 表名 character set gbk;
>    ```
>
> 3. 添加一列
>
>    ```mysql
>    alter table 表名 add 新列名 新数据类型;
>    ```
>
> 4. 修改列名称、数据类型
>
>    ```mysql
>    alter table 表名 change 列名 新列名 新数据类型;
>    ```
>
>    ```mysql
>    alter table 表名 modify 列名 新数据类型;
>    ```
>
> 5. 删除列
>
>    ```mysql
>    alter table 表名 drop 列名;
>    ```
>
> 
>
> ---
>
> ## D [ Delete ] :   删除
>
> 1. 删除一张表
>
>    ```mysql
>    drop table 表名;
>    ```
>
> 2. 先判断是否存在，若存在，再删除
>
>    ```mysql
>    drop table if exists 表名;
>    ```
>
>    ---
>
> ---





---

---





## DML:	增删改表中的数据



> ----
>
> ## 添加数据
>
> 1. 语法：添加新行
>
>    ```mysql
>    insert into 表名(列名1,列名2,...,列名n) values(值1,值2,...,值n);
>    ```
>
> 2. 简化：当添加的行中的列名全部赋值
>
>    ```mysql
>    insert into 表名 values(值1,值2,...,值n);
>    ```
>
> 3. 注意：
>
>    ```mysql
>    # 1: 列名和值 一一对应
>    # 2: 如果表名后,不定义列名,使用2简化过程,要全部赋值
>    # 3: 除了数字类型,其他类型要用引号 [单双引都可以]
>    ```
>
> 
> 
>
> ----
>
> ## 删除数据
>
> 1. 语法：删除某行或某特定值
>
>    ```mysql
>    delete from 表名 [where 条件];
>    delete from stu where name = 'GanGa';
>    ```
>
> 2. 删除全部行
>
>    ```mysql
>    # 第一种方法
>    delete from 表名; #删除该表的所有行
>                                                                                                                                           
>    # 第二种方法
>    truncate table 表名; #先删除这个表,然后再生成一个和之前一样的新表。
>                                                                                                                                           
>    # 推选使用第二种方法,效率更高。
>
> 
> 
>
> ---
>
> ## 修改数据
>
> 1. 语法：修改特定条件的 某列某值
>
>    ```mysql
>    update 表名 set 列名1=值1, 列名2=值2, ...[where 条件];
>    ```
>
> 2. 修改此列全部的值
>
>    ```mysql
>    update 表名 set 列名1=值1, 列名2=值2, ...;
>    ```
>
>    
>    
>
> ----



## DQL：查询表中的记录

>----
>
>1. ## 语法：
>
>```mysql
>select 
>	字段列表
>from
>	表名列表
>where
>	条件列表
>group by
>	分组字段
>having 
>	分组之后的条件
>order by
>	排序
>limit
>	分页限定
>
>
>```
>
>2. ## 基础查询
>
>1. 多个字段的查询
>
>
>```mysql
>select 字段名1,字段名2,... from 表名;
>select * from 表名; #查询所有有字段列表
>```
>
>
>
> 2. 去除重复
>
>    ```mys
>    select distinct 字段名... from 表名;
>    ```
>
>
>
> 3. 计算列
>
>    ```mysql
>    #一般可以使用四则运算技计算一些列的值[一般用于数值计算]
>    #注意: 任何数据运算时,如有有null,结果就为null
>    #使: ifnull(表达式一, 表达式二)
>    #		表达式一: 要判断为null的字段
>    #		表达式二: 为null要替换后的值
>                                                                                                          
>    select
>    	name, -- 姓名
>    	math, -- 数学
>    	nglish, -- 英语
>    	ifnull(math,0)+ifnull(english,0) #总分
>    	from
>    student; -- 学生表
>                                                                                                          
>    ```
>
>
>
> 4.  起别名:
>
>    ```mysql
>    # 字段 as 别名	【或者】	字段 别名
>                                                                                                          
>    select
>    	name as 姓名, -- 姓名
>    	math as 数学, -- 数学
>    	english 英语, -- 英语  可以省略as
>    	ifnull(math,0) +ifnull(english,0) as 总分 -- 总分
>    from
>    	student; -- 学生表
>                                                                                                          
>    ```
>
>---
>
>3. ## 条件查询
>
>1. where子句根条件
>
>2. 运算符
>
>   ```mysql
>    > 、< 、<= 、>= 、= 、<>
>   
>   between ... and
>   
>   in(集合体)
>   
>   like #模糊查询
>   _ # 占位符 单个任意字符
>   % # 占位符 多个任意字符
>   
>   is null
>   
>   and &&
>   or ||
>   not !
>   
>   ```
>
>---
>
>---
>
>3. 演示: 
>
>   ```mysql
>   # 查询年龄大于20岁
>   select * from student where age > 20;
>   # 查询年龄大于等于20岁的
>   select * from student where age >= 20;
>   # 查询年龄不等于20岁的
>   select * from student where age != 20;
>   select * from student where age <> 20;
>   # 查询年龄大于等于20 小于等于30
>   select * from student where age >= 20 && age <= 30;
>   select * from student where age >= 20 and age <= 30;
>   # 使用between ... and ...语句
>   select * from student where age between 20 and 30;
>   # 查询年龄为22岁、18岁、25岁的信息
>   select * from student where age = 22 or age = 18 or age = 25;
>   select * from student where age = 22 || age = 18 || age = 25;
>   # 使用 in()函数
>   select * from student where age in(22,18,25);
>   # 查询英语成绩为null的
>   #select * from student where english = null; #错误语法 null不参与运算和比较、
>   # 使用 is null 语句
>   select * from student where english is null;
>   # 查询英语不为null的
>   select * from student where english is not null;
>   # ==========
>   # 查询姓马的
>   select * from student where name like '马%';
>   # 查询姓马的 且 姓名是两个字的
>   select * from student where name like '马_';
>   # 查询姓马的 且 姓名是三个字的
>   select * from student where name like '马__';
>   # 查询姓名第二个字为 化 的
>   select * from student where name like '_化%';
>   # 查询姓名中含 德 字的
>   select * from student where name like '%德%';
>   # 查询姓名为三字的
>   select * from student where name like '___';
>   ```
>
>---
>
>---
>
>4. ## 排序查询
>
>语法规则
>
>```mysql
>order by 子句
>
>asc  #升序排列 默认就是升序排列 可省略
>desc #降序排列
>
>#多条排序条件
>order by 
>	排序字段1 排序方式1, #先排序方式1
>	排序字段2 排序方式2, #如果1相同 再按方式2
>	......	#以此类推
>
>```
>
>---
>
>---
>
>5. ## 聚合函数：
>
>   1. 概念
>
>      ```mysql
>      # 将一列数据作为一个整体，进行纵向的计算。
>      ```
>
>      
>
>   2. 语法
>
>      ```mysql
>      max #计算最大值
>      min #计算最小值
>      sum #计算值总和
>      avg #计算平均数
>      count #计算个数
>      #count一般选择非空列： 主键
>      #count(*)
>      ```
>
>      
>
>   3. 演示
>
>      ```mysql
>      #聚合函数
>                                                                                                                                                                                
>      # count()
>      # 计算表中所有行的个数
>      select count(name) from student;   # 8
>      select count(english) from student;# 7
>      #count计算的都是非null的个数 因为english中有一个为null所以不进行计算
>                                                                                                                                                                                
>      # 需求1: 班级有多少人 // 需要排除null
>      # 方式一 使用ifnull(函数) // 不推选
>      select count(ifnull(english,0)) as 人数 from student;
>      # 方式二 选择非空的列 // 以后学 //推选
>                                                                                                                                                                                
>      # 需求2: 班级中考了英语的人数
>      select count(english) as 考了英语的人数 from student;
>                                                                                                                                                                                
>      select max(math) as 数学最大分数 from student;
>      select min(id) as 第一个学号 from student;
>      select sum(math) as 数学总分 from student;
>      select avg(math) as 数学平均分 from student;
>      select avg(english) as 英语平均分 from student; #同样不进行计算null
>      select avg(ifnull(english,0)) as 英语平均分 from student;
>      select count(id) 人数, avg(math) 数学平均分 from student; #不止可以使用一个聚合函数
>      ```
>
>      
>
>---
>
>---
>
>6. ## 分组查询
>
>   1. 语法
>
>
>   ```mysql
>   group by 分组字段;
>   ```
>
>   2. 演示与注意：
>
>   ```mysql
>   /*注意:
>   	1.分组之后查询的字段: 分组字段、聚合函数
>   	2.where 和 having 的区别: 
>   		1.where在分组之前进行限定，如果不满足条件，则不参与分组。
>   			having在分组之后进行限定，如果不满足结果，则不会被查询出来。【重点】
>   		2.where后不可以跟聚合函数，having可以进行聚合函数判断。【重点】
>   	3.使用别名代替 having后面的聚合函数
>   */
>   
>   #====================================================
>   
>   # 演示:
>   # 分组查询
>   
>   # 按照性别分组。分别查询男、女同学的平均分
>   select sex , avg(math) from student group by sex;
>   # 1.分组之后查询的字段: 分组字段、聚合函数
>   # 就算跟了也没事 但是毫无意义
>   select name, sex, avg(math) from student group by sex; # 无意义
>   
>   select sex, avg(math), count(id) from student group by sex;
>   
>   # 同上面条件 外加条件: 如果分数低于70分 在不进行参与计算
>   # 在group by 分组之前 加一个where进行判断
>   select sex, avg(math), count(id) from student where math >= 70 group by sex;
>   select sex, avg(math), count(id) from student group by sex;
>   
>   # 附加条件：对结果集进行判断 大于2人的才保留分组
>   # 在group by 分组之后 使用 having + 聚合函数
>   select
>          sex,       -- 分组字段
>          avg(math), -- 求取平均分
>          count(id)  -- 分组总人数
>   from
>        student      -- 学生表
>   where
>         math >= 70  -- 分组判断
>   group by
>            sex      -- 分组列名
>   having count(id) > 2;#分组要求
>   
>   # 改进
>   select
>          sex as 性别,
>          avg(math) as 数学平均分,
>          count(id) as 总人数
>   from
>        student
>   where
>         math >= 70
>   group by
>            sex
>   having #使用别名代替聚合函数 进行判断
>          总人数>2;
>   ```
> ---
>
>  7. ## 分页查询
>
>     1. 语法
>
>          ```mysql
>          limit 开始的索引, 每页查询的条数;
>          ```
>
>     2. 公式
>
>          ```mysql
>          # 开始的索引 = (当前索引 - 1) * 煤业显示的条数 
>          ```
>
>     3. 演示
>
>          ```mysql
>          select * from student limit 0, 3; -- 第一页, 1 2 3行信息
>          select * from student limit 4, 3; -- 第二页, 4 5 6行信息
>          select * from student limit 6, 3; -- 第三页, 7 8 行的信息 // 9行无内容
>          ```
>
>     4. 注意
>
>          ```mysql
>          1.limit 是一个MySQL的"方言"
>          2.其他数据库有z
>          ```
>
>          
>
>
>
>---
>
>



## DCL:	数据控制语言 [了解]

> ## *//TODO:*





---

---





## 约束

> ### 概念：对表中的数据进行限定, 保证数据的正确性、有效性和完整性。

---

>### 分类：
>
>##### 主键约束: primary key
>
>##### 非空约束: not null
>
>##### 唯一约束: unique
>
>##### 外键约束: foreign key







## 非空约束: not null

>1. 概述：
>
>   ```mysql
>   # 创建后 再创建新的行时 必须要给该约束的类型赋值。
>   ```
>
>   
>
>2. 方式一：创建表时 添加非空约束
>
>   ```mysql
>   create table 表名 (
>   			列名1 数据类型1,
>       		列名2 数据类型2 not null,
>       		列名3 数据类型2 not null
>   ); #列名2 和 列名3 是非空约束
>   ```
>
>3. 方式二：创建表之后 添加非空约束
>
>   ```mysql
>   alter table 表名 modify 列名 数据类型 not null;
>   alter table 表名 change 列名 新列名 数据类型 not null;
>   # 注意: 修改时确保数据中没有null值
>   ```
>
>4. 删除非空约束
>
>   ```mysql
>   alter table 表名 modify 列名 数据类型;
>   ```
>
>5. 演示
>
>   ```mysql
>   # 非空约束
>   show tables;
>   create table notNull(
>               id int,
>               name varchar(32) not null -- name为非空
>   );
>   select * from notNull; #查看表
>   
>   insert into notNull (id,name) values(null,null);
>   # 发生错误: Column 'name' cannot be null
>   
>   insert into notnull (id, name) values (null, 'EDG_NB');
>   select * from notnull;
>   
>   # 删除非空约束
>   alter table notnull modify name varchar(32);
>   # 再添加回来
>   alter table notnull modify name varchar(32) not null;
>   alter table notnull change name name varchar(32) not null;
>   # 查询表
>   select * from notnull;
>   ```
>
> ---
>
>---





## 唯一约束: unique

>1. 概述：
>
>   ```mysql
>   # 该列中的值 不能重复
>   ```
>
>2. 方式一: 创建时添加唯一约束
>
>   ```mysql
>   create table 表名(
>   			列名1 数据类型1,
>       		列名2 数据类型2 unique,
>       		列名3 数据类型2 unique
>   ); # 列名2和列名3 是唯一约束
>   ```
>
>3. 方式二: 创建后设置唯一约束
>
>   ```mysql
>   alter table 表名 modify 列名 数据类型 unique;
>   alter table 表名 change 列名 新列名 数据类型 unique;
>   # 注意: 修改时确保数据中没有重复值
>   ```
>
>4. 删除唯一约束 **[注意]**
>
>   ```mysql
>   # 正确格式
>   alter table 表名 drop index 列名;
>   # 错误格式
>   alter table 表名 modify 列名 数据类型;
>   ```
>
>5. 演示
>
>   ```mysql
>   # 唯一约束
>   
>   create table uni(
>            id int,
>            name varchar(32) not null,
>            phone_number varchar(11) unique
>   ); # 手机号唯一约束 unique
>   
>   select * from uni;
>   
>   insert into uni (id, name, phone_number)
>   VALUES (1, 'Gan', '110');
>   
>   insert into uni (id, name, phone_number)
>   VALUES (2, 'Ga', '120');
>   
>   insert into uni (id, name, phone_number)
>   VALUES (3, 'Jiang', '110');
>   # 添加错误 phone_number是唯一的
>   
>   insert into uni (id, name, phone_number)
>   VALUES (4, 'EDG', null); #此列中第一个null 可添加
>   
>   insert into uni (id, name, phone_number)
>   values (5, 'NB', null); #此列中第二个null添加失败, null也算是重复
>   # !!! 关于多个null能否添加有歧义!!!
>   
>   # 删除唯一约束
>   alter table uni modify phone_number varchar(11);
>   # 这样不会报错 但是没有删除唯一约束
>   # 正确格式:
>   alter table uni drop index phone_number;
>   
>   insert into uni (id, name, phone_number)
>   VALUES (3, 'Jiang', '110');
>   
>   # 创建后添加唯一约束
>   # 先删除重复的 110
>   delete from uni where id = 3;
>   select * from uni;
>   
>   # 添加唯一约束
>   alter table uni modify phone_number varchar(11) unique;
>   
>   insert into uni (id, name, phone_number)
>   VALUES (3, 'Jiang', '110'); # 添加失败
>   ```
>
> ---
>
>---







## 主键约束: primary key

>1. 概述
>
>  ```mysql
>  # 含义: 非空且唯一
>  # 一张表中只能有一个字段为主键
>  # 主键就是表中记录的唯一标识
>  ```
>
>
>
>2. 方式一：在创建时添加主键约束
>
>  ```mysql
>  create table 表名(
>  			列名1 数据类型1 primary key,
>      		列名2 数据类型2,
>      		列名3 数据类型3
>  ); # 列名1是主键
>  ```
>
>
>
>3. 方式二：在创建后添加主键约束
>
>  ```mysql
>  alter table 表名 modify 列名 数据类型 primary key;
>  # 注意: 修改时确保数据中没有重复值 且 不为null
>  ```
>
>
>
>4. 删除主键约束  **[注意]**
>
>  ```mysql
>  # 正确格式
>  alter table 表名 drop primary key; 
>  # key后面没有列名, 因为主键就要一个。
>
>  # 错误格式
>  alter table 表名 modify 列名 数据类型;
>  ```
>
>
>
>5. 演示
>
>  ```mysql
>  # 主键约束
>  create table pkey(
>              name varchar(32) not null,
>              phone_number varchar(11) unique,
>              uid int primary key
>  ); # key 是主键
>  select * from pkey;
>
>  insert into pkey (name, phone_number, uid)
>  VALUES ('Gan', '1001', 1); # 添加1
>
>  insert into pkey (name, phone_number, uid)
>  VALUES ('Ga', '1002', 2);  # 添加2
>
>  insert into pkey (name, phone_number, uid)
>  VALUES ('Jiang', '1003', 2); # 主键重复  失败
>
>  insert into pkey (name, phone_number, uid)
>  VALUES ('EDG', '1004', null); # 主键为null  失败
>
>  select * from pkey;
>
>  # 错误的删除主键
>  alter table pkey modify uid int;
>  insert into pkey (name, phone_number, uid)
>  VALUES ('Jiang', '1003', 2); # 依然添加失败
>
>  # 正确的删除主键
>  alter table pkey drop primary key ; # 后面没有 列名 主键就一个
>  insert into pkey (name, phone_number, uid)
>  VALUES ('Jiang', '1003', 2); # 添加成功
>  insert into pkey (name, phone_number, uid)
>  VALUES ('EDG', '1004', null);# 添加成功
>
>  # 先删除重复 且 不为null 的行
>  delete from pkey where name = 'Jiang';
>  delete from pkey where name = 'EDG';
>  # 在添加主键约束
>  alter table pkey modify uid int primary key ;
>  # 添加成功
>
>  ```
>
>
>
>6. 自动增长:
>
>  ```mysql
>  # 概念：
>  # 如果某一列是数值类型的, 
>  # 使用auto_increment 可以来完成值的自动增长。
>
>  create table 表名(
>  		列名 数据类型 primary key auto_increment
>  );
>
>  alter table 表名 id int auto_increment;
>
>  alter table stu modify id int;
>
>  ```
>
>
>
>---











## 外键约束: foreign key

> 初步了解
>
> ```mysql
> # 外键约束
> 
> 
> # ===========表1
> CREATE TABLE emp (
> id INT PRIMARY KEY AUTO_INCREMENT,
> NAME VARCHAR(30),
> age INT,
> dep_name VARCHAR(30),
> dep_location VARCHAR(30)
> );
> -- 添加数据
> INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('张三', 20, '研发部', '广州');
> INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('李四', 21, '研发部', '广州');
> INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('王五', 20, '研发部', '广州');
> INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('老王', 20, '销售部', '深圳');
> INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('大王', 22, '销售部', '深圳');
> INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('小王', 18, '销售部', '深圳');
> select * from emp;
> # 问题: 数据冗余后 且 期还会出现增删改的问题
> 
> 
> # 解决: 通过外键约束
> 
> create table department(
>  id int primary key auto_increment,
>  dep_name varchar(20),
>  dep_location varchar(20)
> );
> 
> 
> create table employee(
>  id int primary key auto_increment,
>  name varchar(20),
>  age int,
>  dep_id int, -- 外键对应主表的主键
> 
>  -- 创建外键约束
>  constraint emp_depid_fk
>  foreign key (dep_id)
>  references department(id)
> );
> -- 添加 2 个部门
> insert into department values(null, '研发部','广州'),(null, '销售部', '深圳');
> -- 正常添加数据
> INSERT INTO employee (NAME, age, dep_id) VALUES ('张三', 20, 1);
> INSERT INTO employee (NAME, age, dep_id) VALUES ('李四', 21, 1);
> INSERT INTO employee (NAME, age, dep_id) VALUES ('王五', 20, 1);
> INSERT INTO employee (NAME, age, dep_id) VALUES ('老王', 20, 2);
> INSERT INTO employee (NAME, age, dep_id) VALUES ('大王', 22, 2);
> INSERT INTO employee (NAME, age, dep_id) VALUES ('小王', 18, 2);
> ```
>
> 
>
> ## 未完 //TODO:
>
> 1. 语法:
>
>    ```mysql
>    # 优先级: 先创建外键1
>    # 表2的外键的表
>    create table 表名1(
>    			列名1 数据类型1 primary key,
>        		列名1 数据类型2,
>        		....
>    );
>
>    # 优先级: 再创建表2 添加外键的表2
>    # 表1要设置外键 
>    create table 表名2(
>    			列名1 数据类型1 primary key,
>        		列名2 数据类型2,
>        		列名3 数据类型3,
>        		......,
>        		# 添加外键
>        		constraint 外键名
>                    foreign key (此表中要设置的外键)
>                    references 要关联的表名1 (外键表1的主键[大多] // 外键表1的非主键)
>    );
>
>    -- 如果要添加的表2时 表2外键值 没有 表1中对应的外键值的话 :
>
>    -- 先添加表1外键表的行
>    -- 再添加表2的行
>    ```
>
> 2. 创建后添加外键
>
>    ```mysql
>    alter table 表名2
>    		add constraint 自定义外键名
>    		foreign key (表名2中要设置外键的列名)
>    		references 要关联的表名1 (外键表1的主键[大多] // 外键表1的非主键);
>
>    # 注意: 要排除约束限制
>    # 表1中的外键值 与 外键表2中所对应的该列名的值,
>    # 必须满足表2值包含所有表1值(只是两个关联的键值)。
>    ```
>
> 3. 删除外键约束
>
>    ```mysql
>    alter table 表名1 drop foreign key 自定义外键名; #外键可能有多个 别忘了加外键名称
>    ```
>
> 4. 注意
>
>    ```my
>    # 外键值可以为null 但是不能时不存在的外键值。
>    ```
>
> 5. 演示
>
>    ```mysql
>    -- =====================
>    -- ===========表1
>       CREATE TABLE emp (
>                id INT PRIMARY KEY AUTO_INCREMENT,
>                NAME VARCHAR(30),
>                age INT,
>                dep_name VARCHAR(30),
>                dep_location VARCHAR(30)
>    );
>    -- 添加数据
>    INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('张三', 20, '研发部', '广州');
>    INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('李四', 21, '研发部', '广州');
>    INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('王五', 20, '研发部', '广州');
>    INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('老王', 20, '销售部', '深圳');
>    INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('大王', 22, '销售部', '深圳');
>    INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('小王', 18, '销售部', '深圳');
>    select * from emp;
>    -- 问题: 数据冗余后 且 期还会出现增删改的问题
>                                                 
>    -- 解决: 通过外键约束
>                                                 
>      create table department(
>                       id int primary key auto_increment,
>                       dep_name varchar(20),
>                       dep_location varchar(20)
>       );
>                                                 
>      create table employee(
>                       id int primary key auto_increment,
>                       name varchar(20),
>                       age int,
>                       dep_id int, -- 外键对应主表的主键
>                        -- 创建外键约束
>                   constraint emp_depid_fk
>                   foreign key (dep_id)
>                   references department(id)
>     );
>       -- 添加 2 个部门
>       insert into department values(null, '研发部','广州'),(null, '销售部', '深圳');
>       -- 正常添加数据
>       INSERT INTO employee (NAME, age, dep_id) VALUES ('张三', 20, 1);
>       INSERT INTO employee (NAME, age, dep_id) VALUES ('李四', 21, 1);
>       INSERT INTO employee (NAME, age, dep_id) VALUES ('王五', 20, 1);
>       INSERT INTO employee (NAME, age, dep_id) VALUES ('老王', 20, 2);
>       INSERT INTO employee (NAME, age, dep_id) VALUES ('大王', 22, 2);
>       INSERT INTO employee (NAME, age, dep_id) VALUES ('小王', 18, 2);
>                                                 
>      -- 单独删除部门中的数据
>       delete from department where id = 1;
>       -- 删除不了 有外键约束
>                                                 
>      -- 添加employee中的数据 dep_id的值时3 外键中的主键没有3
>       insert into employee (name, age, dep_id) VALUES ('王炸', 88, 3);
>       -- 添加失败
>                                                 
>      -- 因该先添加外键表中的主键
>       insert into department values (3, '摸鱼部', '梦里');
>       insert into employee (name, age, dep_id) VALUES ('王炸', 88, 3);
>       -- 添加成功
>       -- ========== 删除外键
>      alter table employee drop foreign key emp_depid_fk;
>      -- 后面别忘了加外键名
>                                                 
>     -- 再次单独删除部门中的数据
>      delete from department where id = 1; #删除成功
>      -- 再次添加employee中的数据 dep_id的值时3 外键中的主键没有3
>      insert into employee (name, age, dep_id) VALUES ('王炸', 88, 3); #添加成功
>      -- 再次向employee中添加不存在的外键值
>      insert into employee (name, age, dep_id) values ('神里', 17, 4); #添加成功
>                                                 
>     -- 查询两种表
>      select * from employee;
>      select * from department;
>                                                 
>     -- ============创建后添加外键
>      alter table employee add constraint emp_dept_fk
>                                  foreign key (dep_id)
>                                  references department(id);
>      -- 此时此刻 是添加不了的
>      -- 先添加外键为1的主键
>      insert into department values (1,'研发部', '广州');
>      -- 或者删除employee表中的外键值中 department表中对应不了的主键
>      delete from employee where id not in(1,2,3);
>                                                 
>     -- 再次尝试添加外键
>      alter table employee add constraint emp_dept_fk
>                                  foreign key (dep_id)
>                                  references department(id);
>      -- 添加成功
>
> 
>
> 
>
>
> 6. ## 级联操作
>
>    1. 发现问题
>
>       ```mysql
>       # 如果修改关联的外键表2中的外键值,由于关联问题,操作起来会很麻烦
>       ```
>
>    2. 解决问题
>
>       ```mysql
>       # 使用级联操作
>       ON UPDATE CASCADE #级联更新
>       ON DELETE CASCADE #级联删除
>
>       alter table 表名1 add constraint 外键名
>       foreign key(外键列)
>       references 表名2(对应外键列)
>       no update cascade
>       no delete cascade;
>       # 连个可以同时添加
>       ```
>
>    3. 注意
>
>       ```mysql
>       # 需要先删除已有的外键 再设置外键 及 级联操作
>       # 演示:
>
>       # 添加级联操作
>       # 先删除外键
>       alter table employee drop foreign key emp_dept_fk;
>       alter table employee add constraint emp_dept_fk
>       foreign key (dep_id)
>       references department(id)
>       on update cascade # 级联更新
>       on delete cascade;# 级联删除
>
>       # 直接修改外表中的主键
>       update department set id = 6 where id = 1; 
>       #修改成功
>
>       # 再次查询两表
>       select * from employee; # 外键也跟着变成了6
>       select * from department;
>       ```
>
>    4. 另外
>
>       ```mysql
>       # 级联操作很危险 用的时候需要谨慎！
>       ```
>
> ---
>
> ---
>
> ---





## ===========





## 数据库的设计





## 一: 多表之间的关系

---

>多表之间的关系
>
>  1. ## 分类
>
>     1. 一对一(了解)：
>
>        ```mysql
>        # 人和身份证
>        # 分析：一个人只有一个身份证，一个身份证只能
>        ```
>
>     2. 一对多(多对一)：
>
>        ```mysql
>        # 如：部门和员工
>        # 分析：一个部门有多个员工，一个员工只能对应一个部门
>        ```
>
>     3. 多对多：
>
>        ```mysql
>        # 如：学生和课程
>        # 分析：一个学生可以选择很多门课程，一个课程也可以被很多学生选择
>        ```
>
>        ---
>
>     ---
>
>  2. ## 实现关系方法：
>
>     1. 一对多(多对一):
>
>     2. ![image-20211118094129415](D:\源代码\img\MDimg\image-20211118094129415.png)
>
>        ```mysql
>        # 如：部门和员工
>        # 实现方式：
>        # 在多的一方建立外键，指向一的一方的主键。
>        foreign key()
>        ```
>
>     3. 多对多:
>
>        ![image-20211118092324995](D:\源代码\img\MDimg\image-20211118092324995.png)
>
>        ```mysql
>        # 学生和课程
>        # 实现方式：
>        # 多对多关系实现需要借助第三张中间表。中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键
>        ```
>
>     4. 一对一(了解):
>
>        ![image-20211118093236560](D:\源代码\img\MDimg\image-20211118093236560.png)
>
>        ```mysql
>        # 如：人和身份证
>        # 实现方式：
>        # 一对一关系实现，可以在任意一方添加唯一外键指向另一方的主键。
>        ```
>
>        ---
>
>     ---
>
>  3. 案例
>
>     ![image-20211118101922960](D:\源代码\img\MDimg\image-20211118101922960.png)
>
>     ```mysql
>     # 表的关系 案例
>     -- ================表1
>     create table tab_category (
>                     cid int primary key auto_increment, -- 分类id 主键 自增
>                     cname varchar(100) not null unique  -- 分类名  非空约束
>     );
>     insert into tab_category (cname) values ('周边游'),
>                                             ('出境游'),
>                                             ('国内游'),
>                                             ('港澳游'); -- 添加分类
>     select * from tab_category; # 查询该表
>                                                                                                     
>     -- ================表2
>     create table tab_route(
>                     rid int primary key auto_increment, -- 旅游线路id 主键 自增
>                     rname varchar(100) not null unique, -- 介绍与线路 非空约束 唯一约束
>                     price double,       -- 价格
>                     rdate date,         -- 日期
>                     cid int,            -- 外键
>                     /*constraint 省略的外键名称*/
>                     foreign key (cid) references tab_category(cid)
>                     -- 表1与表2 设置外键 多对一 多, 外键值可以重复 不能设置唯一约束
>     );
>                                                                                                     
>     INSERT INTO tab_route -- 添加路线
>     VALUES
>     (NULL, '【厦门+鼓浪屿+南普陀寺+曾厝垵 高铁 3 天 惠贵团】尝味友鸭面线 住 1 晚鼓浪屿', 1499, '2018-01-27', 1),
>     (NULL, '【浪漫桂林 阳朔西街高铁 3 天纯玩 高级团】城徽象鼻山 兴坪漓江 西山公园', 699, '2018-02-22', 3),
>     (NULL, '【爆款￥1699 秒杀】泰国 曼谷 芭堤雅 金沙岛 杜拉拉水上市场 双飞六天【含送签费 泰风情 广州 往返 特价团】', 1699, '2018-01-27', 2),
>     (NULL, '【经典?狮航 ￥2399 秒杀】巴厘岛双飞五天 抵玩【广州往返 特价团】', 2399, '2017-12-23',2),
>     (NULL, '香港迪士尼乐园自由行 2 天【永东跨境巴士广东至迪士尼去程交通+迪士尼一日门票+香港如心海景酒店暨会议中心标准房 1 晚住宿】', 799, '2018-04-10', 4);
>     select * from tab_route; # 查询该表
>                                                                                                     
>     -- ================表3
>     create table tab_user (
>                     uid int primary key auto_increment,  -- 用户id 主键
>                     username varchar(100) unique not null,
>                     password varchar(30) not null,
>                     name varchar(100),
>                     birthday date,
>                     sex char(1) default '男',
>                     telephone varchar(11),
>                     email varchar(100)
>     );
>     -- 添加用户数据
>     INSERT INTO tab_user -- 添加用户
>     VALUES
>     (1, 'cz110', 123456, '老王', '1977-07-07', '男', '13888888888', '66666@qq.com'),
>     (2, 'cz119', 654321, '小王', '1999-09-09', '男', '13999999999', '99999@qq.com');
>                                                                                                     
>     select * from tab_user; #查询该表
>                                                                                                     
>     # 现在需要关联 表2 和 表3 多对多的关系
>     # 先创建一个中间表 表2 和 表3 的主键 为字段
>     create table and_user_tab(
>                     rid int, -- 路线id 外键
>                     uid int, -- 用户id 外键
>                     date datetime,
>                     -- 将这两个键设为符合主键
>                     primary key(rid,uid),
>                     -- 分别对应设置外键
>                     foreign key (rid) references tab_route(rid),
>                     foreign key(uid) references tab_user(uid)
>     );
>     insert into and_user_tab  -- 添加数据
>     values
>         (1,1,'2018-01-01'), -- 老王选择厦门
>         (2,1,'2018-02-11'), -- 老王选择桂林
>         (3,1,'2018-03-21'), -- 老王选择泰国
>         (2,2,'2018-04-21'), -- 小王选择桂林
>         (3,2,'2018-05-08'), -- 小王选择泰国
>         (5,2,'2018-06-02'); -- 小王选择迪士尼
>                                                                                                     
>     select * from and_user_tab;
>     ```
>     
>     ---
>     
>      ![tab_category](D:\源代码\img\MDimg\tab_category.png)
>     
>     ---
>
>---
>
>---





---

---





## 二: 数据库设置的范式

>
>
>1. ## 概念：
>
>**设计数据库时，需要遵循的一些规范。要遵循后边的范式要求，必须先遵循前边的所有范式要求**
>
>
>
>2. ## 概述:
>
>设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。目前关系数据库有六种范式：**第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）。**
>
>
>
>3. ## 分类：
>
>1. **第一范式（1NF）**：每一列都是不可分割的原子数据项
>
>  >>> ## 图 
>  >>>![image-20211119091140299](D:\源代码\img\MDimg\image-20211119091140299.png)
>  >>> 
>
>---
>
>
>
>---
>
>
>
>2. **第二范式（2NF）**：在1NF的基础上，非码属性必须完全依赖于码（在1NF基础上消除非主属性对主码的部分函数依赖）
>
>  ```mysql
>  /*几个概念：
>
>  1. 函数依赖：A-->B,如果通过A属性(属性组)的值，可以确定唯一B属性的值。则称B依赖于A
>     例如：学号-->姓名。  （学号，课程名称） --> 分数
>
>  2. 完全函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定需要依赖于A属性组中所有的属性值。
>     例如：（学号，课程名称） --> 分数
>
>  3. 部分函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定只需要依赖于A属性组中某一些值即可。
>     例如：（学号，课程名称） -- > 姓名
>
>  4. 传递函数依赖：A-->B, B -- >C . 如果通过A属性(属性组)的值，可以确定唯一B属性的值，
>     在通过B属性（属性组）的值可以确定唯一C属性的值，则称 C 传递函数依赖于A
>     例如：学号-->系名，系名-->系主任
>
>  5. 码：如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性(属性组)为该表的码
>     例如：该表中码为：（学号，课程名称）
>          -- 主属性：码属性组中的所有属性
>  		-- 非主属性：除过码属性组的属性
>
>  */
>  ```
>
>  > >> ## 图
>  > >>
>  > >>![image-20211119093221509](D:\源代码\img\MDimg\image-20211119093221509.png)
>  > >
>  > >---
>  >
>  > ---
>
>
>
>---
>
>
>
>
> 3. **第三范式（3NF）**：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）
>
>>>>## 图
>>
>>>>![image-20211122084919844](D:\源代码\img\MDimg\image-20211122084919844.png)
>>>
>>>---
>>
>>---
>
>---







## 数据库备份

>## 两种方式:
>
>1. 命令行
>
>   1. 备份
>
>      ```cmd
>      mysqldump -u用户名 -p密码 数据库名称 > 保存的路径
>      ```
>
>   2. 还原
>
>       1. 登录数据库
>
>       2. 创建数据库
>
>       3. 使用数据库
>
>       4. 执行文件。source 文件路径
>
>          ```mysql
>          source 文件路径/文件名.sql
>          ```
>
> ---
>
> ---
>
>   
>
>   
>
>2. 图形化工具
>
>
>
>
>
>





## ===========





## 多表查询

>---
>
>## 概述
>
>两张表查询 `select * from 表1, 表2;` 的到的行数为***笛卡尔积***
>
>``笛卡尔积: 有两个集合A, B。 取这两个集合中所有组成情况。``
>
>但是，有些信息是不需要或者错误的，
>
>多表查询就是为了**将笛卡尔积中不需要或者错误的信息给去除。**
>
>
>
>## 两表的案例表
>
>```mysql
># 多表查询
>
># =========案例表
># 创建部门表
>CREATE TABLE dept(
> id INT PRIMARY KEY AUTO_INCREMENT,
> NAME VARCHAR(20)
>);
># 添加部门
>INSERT INTO dept (NAME) VALUES ('开发部'),('市场部'),('财务部');
>
># 创建员工表
>CREATE TABLE emp (
> id INT PRIMARY KEY AUTO_INCREMENT,
> NAME VARCHAR(10),
> gender CHAR(1), -- 性别
> salary DOUBLE, -- 工资
> join_date DATE, -- 入职日期
> dept_id INT,
> FOREIGN KEY (dept_id) REFERENCES dept(id) -- 外键，关联部门表(部门表的主键)
>);
># 添加员工
>INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('孙悟空','男',7200,'2013-02-24',1);
>INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('猪八戒','男',3600,'2010-12-02',2);
>INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('唐僧','男',9000,'2008-08-08',2);
>INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('白骨精','女',5000,'2015-10-07',3);
>INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('蜘蛛精','女',4500,'2011-03-14',1);
>```
>
>
>
>
>
>## 多表查询分类
>
>> ---
>
>> > ---
>
>> > > ---
>
>## 分类一: 内连接查询
>
>内连接查询的使用:
>
>1. 从哪些表中查询数据
>2. 条件是什么
>3. 查询哪些字段
>4. 条件一般是对应外键值相等, 如 `emp.dept_id = dept.id`
>
>
>
>
>
>## 1.1 隐式内连接
>
>**隐式内连接：使用where条件消除无用数据**
>
>
>
>1. 语法
>
>  ```mysql
>  select 
>  	表名1.字段1,
>  	表名1.字段2,
>  	...
>  	表名2.字段1,
>  	表名2.字段2,
>  	...
>  	...
>  from
>  	表名1,
>  	表名2,
>  	...
>  where
>  	条件判断;
>
>  #改进
>  select
>  	别名1.字段1,
>  	别名1.字段2,
>  	...
>  	别名2.字段1,
>  	别名2.字段2,
>  	...
>  	...
>  from
>  	表名1 别名1,
>  	表名2 别名2,
>  where
>  	条件判断;
>  ```
>
>2. 演示
>
>  ```mysql
>  # 内连接查询
>  # 隐式内连接：使用where条件消除无用数据
>  select * from emp, dept where emp.dept_id = dept.id;
>  #查询员工id 姓名 和所在部门
>
>  select
>    t1.id,  -- 员工id
>    t1.name,-- 员工姓名
>    t2.name -- 员工部门
>  from
>    emp t1,
>    dept t2
>  where
>    t1.dept_id = t2.id;
>  ```
>
>---
>
>---
>
>---
>
>## 1.2显示内连接
>
>1. 语法
>
>  ```mysql
>  select 
>  	字段列表
>  from
>  	表名1
>  inner join  -- inner可以省略
>  	表名2
>  on 
>  	条件;
>
>  #改进
>
>  select 
>  	字段列表
>  from
>  	表名1 别名1
>  inner join	-- inner可以省略
>  	表名2 别名2
>  on
>  	条件;
>  ```
>
>2. 演示
>
>  ```mysql
>  # 内连接查询
>  # 显示内连接查询
>  select * from emp inner join dept on emp.dept_id = dept.id;
>
>  #查询员工id 姓名 和所在部门
>  select
>      t1.id,  -- 员工id
>      t1.name,-- 员工姓名
>      t2.name -- 员工部门
>  from
>      emp t1
>  inner join
>      dept t2
>  on t1.dept_id = t2.id;
>  ```
>
>---
>
>---
>
>---
>
>
>
>## 分类二：外连接查询
>
>
>
>## 2.1 左外连接:
>
>1. 概述
>
>  ```mysql
>  # 查询的是左表所有有数据以及其交集部分。
>  ```
>
>2. 语法
>
>  ```mysql
>  select 
>  	字段列表
>  from
>  	表1
>  left outer join -- outer可以省略
>  	表2
>  on 
>  	条件;
>
>  #表1中所有数据
>  #表2中与满足条件的部分条件(交集)
>
>  #改进: 同样可以起别名 >>>
>  ```
>
>3. 演示
>
>  ```mysql
>  #外连接查询
>  #左外连接查询
>  #现在新来了个员工 雷震子 还没有部门
>
>  insert into emp
>  values(
>         6,
>         '雷震子',
>         '男',
>         null,
>         null,
>         null
>  );
>  #现在要查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称
>  #表1为左-->所有数据, 表2为交集
>
>  select
>      t1.*,
>      t2.*
>  from
>      emp t1
>  left outer join
>      dept t2
>  on
>      t1.dept_id = t2.id;
>  ```
>
>---
>
>---
>
>---
>
>## 2.2 右外连接:
>
>1. 概述
>
>  ```mysql
>  # 查询的是右表所有有数据以及其交集部分。
>  ```
>
>2. 语法
>
>  ```mysql
>  select 
>  	字段列表
>  from
>  	表1
>  right outer join
>  	表2
>  on
>  	条件;
>
>  #表2中所有数据
>  #表1中与满足条件的部分条件(交集)
>
>  #改进: 同样可以起别名 >>>
>  ```
>
>3. 演示
>
>  ```mysql
>  # 外连接查询
>  # 右外连接查询
>  #现在先添加了一个部门 为 删库跑路部
>
>  insert into
>  dept(NAME)
>  values ('删库跑路部');
>
>  #现在要查询所有部门中人员信息，部门有员工就显示员工, 没员工为null ，员工在左边列表，部门在右边位置
>  select
>      t1.*,
>      t2.*
>  from
>      emp t1
>  right outer join
>      dept t2
>  on t1.dept_id = t2.id
>  ```
>
>---
>
>---
>
>---
>
>
>
>## 分类三：子查询
>
>1. 概念
>
>```mysql
># 查询中嵌套查询, 称嵌套查询为子查询
>```
>
>2. 例如
>
>```mysql
>-- 查询工资最高的员工信息
>
>-- 1 查询最高的工资是多少 9000
>SELECT MAX(salary) FROM emp;	
>-- 2 查询员工信息，并且工资等于9000的
>SELECT * FROM emp WHERE salary = 9000;
>
>-- 一条sql就完成这个操作。子查询
>SELECT * FROM emp 
>WHERE salary = (SELECT MAX(salary) FROM emp);
>```
>
>---
>
>
>
>
>
>## 根据子查询不同情况
>
>> ## 一般给一个别名
>
>## 3.1 子查询的结果是单行单列的:
>
>```mysql
>#子查询可以作为条件，使用运算符去判断。 运算符： > >= < <= =
>
>#例:查询员工工资小于平均工资的人
>SELECT * FROM emp WHERE emp.salary < (SELECT AVG(salary) FROM emp);
>```
>
>---
>
>## 3.2 子查询的结果是多行单列的：
>
>```mysql
>-- 查询'财务部'和'市场部'所有的员工信息
>-- 先查看id
>SELECT id FROM dept WHERE NAME = '财务部' OR NAME = '市场部';
>-- 通过id得到员工信息
>SELECT * FROM emp WHERE dept_id = 3 OR dept_id = 2;
>
>-- 子查询
>-- 子查询可以作为条件，使用运算符in来判断
>SELECT * FROM emp WHERE dept_id IN (SELECT id FROM dept WHERE NAME = '财务部' OR NAME = '市场部');
>```
>
>---
>
>## 3.1 子查询的结果是多行多列的：
>
>```mysql
>-- 查询员工入职日期是2011-11-11日之后的员工信息和部门信息
>-- 子查询
>-- 子查询可以作为一张虚拟表参与查询
>select * from dept 
>			t1 ,
>			(select * from emp where emp.`join_date` > '2011-11-11') t2 -- 子查询可以作为一张虚拟表参与查询
>where t1.id = t2.dept_id;
>
>-- 普通内连接也是可以的 --> 隐式内连接
>SELECT 
>	* 
>FROM
>	emp t1,dept t2
>WHERE 
>t1.`dept_id` = t2.`id` AND t1.`join_date` >  '2011-11-11'
>```
>
>---
>
>---
>
>---





## 多表查询练习



### 题目表

```mysql
-- 部门表
CREATE TABLE dept (
                id INT PRIMARY KEY PRIMARY KEY, -- 部门id
                dname VARCHAR(50), -- 部门名称
                loc VARCHAR(50) -- 部门所在地
);

-- 添加4个部门
INSERT INTO dept(id,dname,loc)
 VALUES
    (10,'教研部','北京'),
    (20,'学工部','上海'),
    (30,'销售部','广州'),
    (40,'财务部','深圳');



-- 职务表，职务名称，职务描述
CREATE TABLE job (
                id INT PRIMARY KEY,
                jname VARCHAR(20),
                description VARCHAR(50)
);

-- 添加4个职务
INSERT INTO job (id, jname, description)
 VALUES
    (1, '董事长', '管理整个公司，接单'),
    (2, '经理', '管理部门员工'),
    (3, '销售员', '向客人推销产品'),
    (4, '文员', '使用办公软件');




-- 员工表
CREATE TABLE emp (
                id INT PRIMARY KEY, -- 员工id
                ename VARCHAR(50), -- 员工姓名
                job_id INT, -- 职务id
                mgr INT , -- 上级领导
                joindate DATE, -- 入职日期
                salary DECIMAL(7,2), -- 工资
                bonus DECIMAL(7,2), -- 奖金
                dept_id INT, -- 所在部门编号
                CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY (job_id) REFERENCES job (id),
                CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept (id)
);

-- 添加员工
INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id)
VALUES
(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);



-- 工资等级表
CREATE TABLE salarygrade (
                    grade INT PRIMARY KEY,   -- 级别
                    losalary INT,  -- 最低工资
                    hisalary INT -- 最高工资
);

-- 添加5个工资等级
INSERT INTO salarygrade(grade,losalary,hisalary)
VALUES
    (1,7000,12000),
    (2,12010,14000),
    (3,14010,20000),
    (4,20010,30000),
    (5,30010,99990);



-- 需求：

-- 1.查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述

-- 2.查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置

-- 3.查询员工姓名，工资，工资等级

-- 4.查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级

-- 5.查询出部门编号、部门名称、部门位置、部门人数

-- 6.查询所有员工的姓名及其直接上级的姓名,没有领导的员工也需要查询
```



### 问题一

```mysql
-- 1.查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述
/*
    分析：
        1.员工编号，员工姓名，工资，需要查询emp表  职务名称，职务描述 需要查询job表
        2.查询条件 emp.job_id = job.id
*/
SELECT
    t1.`id`, -- 员工编号
    t1.`ename`, -- 员工姓名
    t1.`salary`,-- 工资
    t2.`jname`, -- 职务名称
    t2.`description` -- 职务描述
FROM
    emp t1, job t2
WHERE
        t1.`job_id` = t2.`id`;
```



### 问题二

```mysql
-- 2.查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置
/*
    分析：
        1. 员工编号，员工姓名，工资 emp  职务名称，职务描述 job  部门名称，部门位置 dept
        2. 条件： emp.job_id = job.id and emp.dept_id = dept.id
*/

SELECT
    t1.`id`, -- 员工编号
    t1.`ename`, -- 员工姓名
    t1.`salary`,-- 工资
    t2.`jname`, -- 职务名称
    t2.`description`, -- 职务描述
    t3.`dname`, -- 部门名称
    t3.`loc` -- 部门位置
FROM
    emp t1, job t2,dept t3
WHERE
        t1.`job_id` = t2.`id` AND t1.`dept_id` = t3.`id`;
```



### 问题三

```mysql
-- 3.查询员工姓名，工资，工资等级
/*
    分析：
        1.员工姓名，工资 emp  工资等级 salarygrade
        2.条件 emp.salary >= salarygrade.losalary and emp.salary <= salarygrade.hisalary
            emp.salary BETWEEN salarygrade.losalary and salarygrade.hisalary
			*/
SELECT
    t1.ename ,
    t1.`salary`,
    t2.*
FROM emp t1, salarygrade t2
WHERE t1.`salary` BETWEEN t2.`losalary` AND t2.`hisalary`;
```



### 问题四

```mysql
-- 4.查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级
/*
    分析：
        1. 员工姓名，工资 emp ， 职务名称，职务描述 job 部门名称，部门位置，dept  工资等级 salarygrade
        2. 条件： emp.job_id = job.id and emp.dept_id = dept.id and emp.salary BETWEEN salarygrade.losalary and salarygrade.hisalary

*/
SELECT
    t1.`ename`,
    t1.`salary`,
    t2.`jname`,
    t2.`description`,
    t3.`dname`,
    t3.`loc`,
    t4.`grade`
FROM
    emp t1,job t2,dept t3,salarygrade t4
WHERE
        t1.`job_id` = t2.`id`
  AND t1.`dept_id` = t3.`id`
  AND t1.`salary` BETWEEN t4.`losalary` AND t4.`hisalary`;
```



### 问题五

```mysql
-- 5.查询出部门编号、部门名称、部门位置、部门人数
/*
    分析：
        1.部门编号、部门名称、部门位置 dept 表。 部门人数 emp表
        2.使用分组查询。按照emp.dept_id完成分组，查询count(id)
        3.使用子查询将第2步的查询结果和dept表进行关联查询

*/
SELECT
    t1.`id`,t1.`dname`,t1.`loc` , t2.total
FROM
    dept t1,
    (SELECT
         dept_id,COUNT(id) total
     FROM
         emp
     GROUP BY dept_id) t2
WHERE t1.`id` = t2.dept_id;

```



### 问题六

```mysql
-- 6.查询所有员工的姓名及其直接上级的姓名,没有领导的员工也需要查询
/*
    分析：
        1.姓名 emp， 直接上级的姓名 emp
            * emp表的id 和 mgr 是自关联
        2.条件 emp.id = emp.mgr
        3.查询左表的所有数据，和 交集数据
            * 使用左外连接查询


select
    t1.ename,
    t1.mgr,
    t2.`id`,
    t2.ename
from emp t1, emp t2
where t1.mgr = t2.`id`;

*/

SELECT
    t1.ename,
    t1.mgr,
    t2.`id`,
    t2.`ename`
FROM
     emp t1
LEFT JOIN
    emp t2
ON
    t1.`mgr` = t2.`id`;

```







## ===========





## 事务

>## 事务介绍
>
>1. 概念
>
>   如果一个包含多个步骤的业务操作, 被事务管理,`那么这个操作要么成功, 要么失败。`
>
> ---
>
>2. 操作
>
>   1. 开启事务: `start transaction;`
>   2. 回滚：     `rollback;`
>   3. 提交：     `commit;`
>
> ---
>
>3. MySQL数据库中事务默认自动提交
>
>   事务提交的两种方式：
>
>   1. 自动提交：
>      1. mysql就是自动提交的
>      2. 一条DML(增删改)语句会自动提交一次事务。
>   2. 手动提交:
>      1. Oracle 数据库默认是手动提交事务
>      2. 需要先开启事务，再提交
>   3. **修改事务的默认提交方式**：
>      1. 查看事务的默认提交方式：`select @@autocommit;`
>      2. 修改默认提交方式：`set @@autocommit = 0; -0或1`
>      3. 其中：`1 代表自动提交  0 代表手动提交`
>
> ---
>
>---

> ## 事务的四大特征
>
> 1. `原子性：` 是不可分割的最小操作单位，要么同时成功，要么同时失败。
> 2. `持久性：` 当事务提交或回滚后，数据库会持久化的保存数据。
> 3. `隔离性：` 多个事务之间。互相独立。
> 4. `一致性：` 事务操作前后，数据总量不变
>
> ---
>
> ---

> ## 事务的隔离级别（了解）
>
> 1. **概念：**多个事务之间隔离的，相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。
>
> 2. **存在问题：**
>
>    1. **`脏读：`***一个事务，读取到另一个事务中没有提交的数据
>    2. **`不可重复读(虚读)：`**在同一个事务中，两次读取到的数据不一样。
>    3. **` 幻读：`**一个事务操作(DML)数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。
>
> 3. **隔离级别：**
>
>    1. `read uncommitted：` **读未提交**
>
>       ​									产生的问题：脏读、不可重复读、幻读
>
>       
>
>    2. `read committed：` **读已提交 （Oracle）**
>
>       ​								 产生的问题：不可重复读、幻读
>
>       
>
>    3. `repeatable read：` **可重复读 （MySQL默认）**
>
>       ​								  产生的问题：幻读
>
>       
>
>    4. `serializable：` **串行化** 
>
>       ​							可以解决所有的问题
>
> 4. **注意:**
>
>    **隔离级别从小到大安全性越来越高，但是效率越来越低**
>
>    数据库查询隔离级别：`select @@tx_isolation;`
>
>    数据库设置隔离级别：`set global transaction isolation level  级别字符串;`
>
> 5. **演示：**
>
>    ```mysql
>    set global transaction isolation level read uncommitted;
>    start transaction;
>                                           
>    -- 转账操作
>    update account set balance = balance - 500 where id = 1;
>    update account set balance = balance + 500 where id = 2;
>    ```
>
>    ---
>
> ---








## ===========









## TODO:

