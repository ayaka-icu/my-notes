# 🌸命令语法--备忘



## MySQL语法



### DDL - 操作数据库、表



Create: 创建数据库 / 表:

```mysql
# 创建数据库：
create database 数据库名称;
# 创建数据库, 判断不纯在, 在创建：
create database if not exists 数据库名称;
# 创建数据库, 并且指定字符集：
create database 数据库名称 character set 字符集名称;
# 创建newgbk数据库, 判断是否存在, 指定gbk编码
create database if not exists newgbk character gbk;

# 创建表
create table 表名(
	列名1 数据类型1, #用逗号隔开
	列名2 数据类型2(...),
	..... i
	列名n 数据类型n  #最后一个不带逗号
);	#最后别忘了分号结束语句
# 案例 - 创建一个表
create table 数据类型( #创建一个表,格式: create table 表名称(  ... );
    字符串 varchar(10), #创建列名称和数据类型：表名称 数据类型 , ...
    整形 int,     #多个列用 , 隔开
    小数类型1 double, #double()
    小数类型2 double(5,2), #设置范围,最大值时99999.99
    日期1 date,       #日期 只包含年月日 yyyy-MM-dd
    日期2 datetime,   #日期 包含年月日时分秒,yyyy-MM-dd HH:mm:ss
    日期3 timestamp,  #日期 获取当前系统日期时间
    布尔值 boolean,
    还有一些不常用的 text,
    end int      #最后一个列不用 , 号，否则会报错
);               #最后别忘了括号外面也有一个分号用于结束该整条语句。
# 另外的：copy到新表
create table 新表名 like 被复制的表名;
```

Retrieve: 查询数据库 / 表:

```mysql
# 查询所有数据库的名称:
show databases;
# 查询某个数据库的字符集:查询某个数据库的创建语
show create database 数据库名称;

# 查询某个数据库中所有的表名称
show tables;
# 查询表结构
desc 表名称;
```

alter: 修改数据库 / 表:

```mysql
# 修改数据库的字符集
alter database 数据库名称 character set 字符集名称;

# 修改表名
alter table 表名 rename to 新表名;
# 修改表的字符集
alter table 表名 character set gbk;
# 添加一列
alter table 表名 add 新列名 新数据类型;
# 修改列名称、数据类型
alter table 表名 change 列名 新列名 新数据类型;
alter table 表名 modify 列名 新数据类型;
# 删除列
alter table 表名 drop 列名;
```

drop: 删除数据库 / 表:

```mysql
# drop database 数据库名称;
drop database 数据库名称;
# 判断数据库存在，存在再删除
drop database if exists 数据库名称;

# 删除一张表
drop table 表名;
# 先判断是否存在，若存在，再删除
drop table if exists 表名;
```

使用数据库

```mysql
# 查询当前正在使用的数据库名称
select databese();		#要带括号 
# 使用数据库
use 数据库名称;
```





### DML - 增删改表中数据



添加数据：

```mysql
# 添加新行
insert into 表名(列名1,列名2,...,列名n) values(值1,值2,...,值n);
# 当添加的行中的列名全部赋值
insert into 表名 values(值1,值2,...,值n);
# 注意：
# 1: 列名和值 一一对应
# 2: 如果表名后,不定义列名,使用2简化过程,要全部赋值
# 3: 除了数字类型,其他类型要用引号 [单双引都可以]
```

删除数据:

```mysql
# 删除某行或某特定值
delete from 表名 [where 条件];
delete from stu where name = 'GanGa';

# 第一种方法
delete from 表名; #删除该表的所有行
# 第二种方法
truncate table 表名; #先删除这个表,然后再生成一个和之前一样的新表。

# 推选使用第二种方法,效率更高。
```

修改数据:

```mysql
# 修改特定条件的 某列某值
update 表名 set 列名1=值1, 列名2=值2, ...[where 条件];
# 修改此列全部的值
update 表名 set 列名1=值1, 列名2=值2, ...;
```





### DQL - 查询表中的记录



基本语法:

```mysql
# 语法
select 
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段
having 
	分组之后的条件
order by
	排序
limit
	分页限定
```



基础查询：

```mysql
# 多个字段的查询
select 字段名1,字段名2,... from 表名;
select * from 表名; #查询所有有字段列表

# 去除重复
select distinct 字段名... from 表名;

# 计算列
#一般可以使用四则运算技计算一些列的值[一般用于数值计算]
#注意: 任何数据运算时,如有有null,结果就为null
#使: ifnull(表达式一, 表达式二)
#		表达式一: 要判断为null的字段
#		表达式二: 为null要替换后的值
select
	name, -- 姓名
	math, -- 数学
	nglish, -- 英语
	ifnull(math,0)+ifnull(english,0) #总分
	from
student; -- 学生表

# 起别名:
# 字段 as 别名	【或者】	字段 别名
select
	name as 姓名, -- 姓名
	math as 数学, -- 数学
	english 英语, -- 英语  可以省略as
	ifnull(math,0) +ifnull(english,0) as 总分 -- 总分
from
	student; -- 学生表
```

条件查询:

```mysql
# where子句根条件
# 运算符：
> 、< 、<= 、>= 、= 、<>

between ... and

in(集合体)

like #模糊查询
_ # 占位符 单个任意字符
% # 占位符 多个任意字符

is null

and &&
or ||
not !

# 查询年龄大于20岁
select * from student where age > 20;
# 查询年龄大于等于20岁的
select * from student where age >= 20;
# 查询年龄不等于20岁的
select * from student where age != 20;
select * from student where age <> 20;
# 查询年龄大于等于20 小于等于30
select * from student where age >= 20 && age <= 30;
select * from student where age >= 20 and age <= 30;
# 使用between ... and ...语句
select * from student where age between 20 and 30;
# 查询年龄为22岁、18岁、25岁的信息
select * from student where age = 22 or age = 18 or age = 25;
select * from student where age = 22 || age = 18 || age = 25;
# 使用 in()函数
select * from student where age in(22,18,25);
# 查询英语成绩为null的
#select * from student where english = null; #错误语法 null不参与运算和比较、
# 使用 is null 语句
select * from student where english is null;
# 查询英语不为null的
select * from student where english is not null;
# ==========
# 查询姓马的
select * from student where name like '马%';
# 查询姓马的 且 姓名是两个字的
select * from student where name like '马_';
# 查询姓马的 且 姓名是三个字的
select * from student where name like '马__';
# 查询姓名第二个字为 化 的
select * from student where name like '_化%';
# 查询姓名中含 德 字的
select * from student where name like '%德%';
# 查询姓名为三字的
select * from student where name like '___';
```

排序查询:

```mysql
order by 子句

asc  #升序排列 默认就是升序排列 可省略
desc #降序排列

#多条排序条件
order by 
	排序字段1 排序方式1, #先排序方式1
	排序字段2 排序方式2, #如果1相同 再按方式2
	......	#以此类推
```

聚合函数:

```mysql
# 将一列数据作为一个整体，进行纵向的计算。
max #计算最大值
min #计算最小值
sum #计算值总和
avg #计算平均数
count #计算个数
#count一般选择非空列： 主键
#count(*)
#聚合函数

# count()
# 计算表中所有行的个数
select count(name) from student;   # 8
select count(english) from student;# 7
#count计算的都是非null的个数 因为english中有一个为null所以不进行计算

# 需求1: 班级有多少人 // 需要排除null
# 方式一 使用ifnull(函数) // 不推选
select count(ifnull(english,0)) as 人数 from student;
# 方式二 选择非空的列 // 以后学 //推选

# 需求2: 班级中考了英语的人数
select count(english) as 考了英语的人数 from student;

select max(math) as 数学最大分数 from student;
select min(id) as 第一个学号 from student;
select sum(math) as 数学总分 from student;
select avg(math) as 数学平均分 from student;
select avg(english) as 英语平均分 from student; #同样不进行计算null
select avg(ifnull(english,0)) as 英语平均分 from student;
select count(id) 人数, avg(math) 数学平均分 from student; #不止可以使用一个聚合函数
```

分组查询:

```mysql
# 语法
group by 分组字段;
/*注意:
1.分组之后查询的字段: 分组字段、聚合函数
2.where 和 having 的区别: 
  1.where在分组之前进行限定，如果不满足条件，则不参与分组。
  		having在分组之后进行限定，如果不满足结果，则不会被查询出来。【重点】
  2.where后不可以跟聚合函数，having可以进行聚合函数判断。【重点】
3.使用别名代替 having后面的聚合函数
*/

# 演示 分组查询:
# 按照性别分组。分别查询男、女同学的平均分
select sex , avg(math) from student group by sex;
# 1.分组之后查询的字段: 分组字段、聚合函数
# 就算跟了也没事 但是毫无意义
select name, sex, avg(math) from student group by sex; # 无意义

select sex, avg(math), count(id) from student group by sex;

# 同上面条件 外加条件: 如果分数低于70分 在不进行参与计算
# 在group by 分组之前 加一个where进行判断
select sex, avg(math), count(id) from student where math >= 70 group by sex;
select sex, avg(math), count(id) from student group by sex;

# 附加条件：对结果集进行判断 大于2人的才保留分组
# 在group by 分组之后 使用 having + 聚合函数
select
       sex,       -- 分组字段
       avg(math), -- 求取平均分
       count(id)  -- 分组总人数
from
     student      -- 学生表
where
      math >= 70  -- 分组判断
group by
         sex      -- 分组列名
having count(id) > 2;#分组要求

# 改进
select
       sex as 性别,
       avg(math) as 数学平均分,
       count(id) as 总人数
from
     student
where
      math >= 70
group by
         sex
having #使用别名代替聚合函数 进行判断
       总人数>2;
```

分页查询:

```mysql
# 语法
limit 开始的索引, 每页查询的条数;

# 开始的索引 = (当前索引 - 1) * 煤业显示的条数 

select * from student limit 0, 3; -- 第一页, 1 2 3行信息
select * from student limit 4, 3; -- 第二页, 4 5 6行信息
select * from student limit 6, 3; -- 第三页, 7 8 行的信息 // 9行无内容

# limit 是一个MySQL的"方言"
```







### 多表查询

隐式内连接:

```mysql
# 隐式内连接：使用where条件消除无用数据
select
	别名1.字段1,
	别名1.字段2,
	...
	别名2.字段1,
	别名2.字段2,
	...
	...
from
	表名1 别名1,
	表名2 别名2,
where
	条件判断;
	
# 隐式内连接：使用where条件消除无用数据
select * from emp, dept where emp.dept_id = dept.id;

# 查询员工id 姓名 和所在部门
select
  t1.id,  -- 员工id
  t1.name,-- 员工姓名
  t2.name -- 员工部门
from
  emp t1,
  dept t2
where
  t1.dept_id = t2.id;
```

显示内连接:

```mysql
select 
	字段列表
from
	表名1
inner join  -- inner可以省略
	表名2
on 
	条件;

# 改进
select 
	字段列表
from
	表名1 别名1
inner join	-- inner可以省略
	表名2 别名2
on
	条件;
	
# 显示内连接查询
select * from emp inner join dept on emp.dept_id = dept.id;

#查询员工id 姓名 和所在部门
select
    t1.id,  -- 员工id
    t1.name,-- 员工姓名
    t2.name -- 员工部门
from
    emp t1
inner join
    dept t2
on t1.dept_id = t2.id;
```

左外连接查询：

```mysql
# 查询的是左表所有有数据以及其交集部分。
select 
	字段列表
from
 	表1
left outer join -- outer可以省略
 	表2
on 
 	条件;

# 表1中所有数据
# 表2中与满足条件的部分条件(交集)
# 改进: 同样可以起别名 >>>
```

右外连接查询:

```mysql
# 查询的是右表所有有数据以及其交集部分。
select 
	字段列表
from
	表1
right outer join
	表2
on
	条件;

# 表2中所有数据
# 表1中与满足条件的部分条件(交集)
```

子查询:

```mysql
# 查询中嵌套查询, 称嵌套查询为子查询
-- 查询工资最高的员工信息

-- 1 查询最高的工资是多少 9000
SELECT MAX(salary) FROM emp;	
-- 2 查询员工信息，并且工资等于9000的
SELECT * FROM emp WHERE salary = 9000;

-- 一条sql就完成这个操作。子查询
SELECT * FROM emp 
WHERE salary = (SELECT MAX(salary) FROM emp);
```











### 表创建 与 约束



```mysql
CREATE TABLE `tb_hotel` (
   # NOT NULL 非空约束 
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '酒店id', # AUTO_INCREMENT 自增长
  `name` varchar(255) NOT NULL COMMENT '酒店名称',		# COMMENT 注释
  `address` varchar(255) NOT NULL COMMENT '酒店地址',   
  `price` int(10) NOT NULL COMMENT '酒店价格',
  `score` int(2) NOT NULL COMMENT '酒店评分',
  `brand` varchar(32) NOT NULL UNIQUE COMMENT '酒店品牌', # UNIQUE 唯一约束
  `city` varchar(32) NOT NULL COMMENT '所在城市',
  `star_name` varchar(16) DEFAULT NULL COMMENT '酒店星级，1星到5星，1钻到5钻',
  `business` varchar(255) DEFAULT NULL COMMENT '商圈',	# DEFAULT NULL 默认值为null
  `latitude` varchar(32) NOT NULL COMMENT '纬度',
  `longitude` varchar(32) NOT NULL COMMENT '经度',
  `pic` varchar(255) DEFAULT NULL COMMENT '酒店图片',
  PRIMARY KEY (`id`) USING BTREE		# PRIMARY KEY 主键约束
) ENGINE=InnoDB AUTO_INCREMENT=2062643523 DEFAULT CHARSET=utf8mb4 ROW_FORMAT=COMPACT;

# NOT NULL 非空约束 
# AUTO_INCREMENT 自增长
# UNIQUE 唯一约束
# COMMENT 注释
# DEFAULT NULL 默认值为null
# PRIMARY KEY 主键约束
```

约束相关语法

```mysql
# 非空约束：not null
# 创建表之后 添加非空约束 
# 注意: 修改时确保数据中没有null值
alter table 表名 modify 列名 数据类型 not null;
alter table 表名 change 列名 新列名 数据类型 not null;
# 删除非空约束
alter table 表名 modify 列名 数据类型;


# 唯一约束：unique
# 创建后设置唯一约束
# 注意: 修改时确保数据中没有重复值
alter table 表名 modify 列名 数据类型 unique;
alter table 表名 change 列名 新列名 数据类型 unique;
# 删除唯一约束 [注意]
# 正确格式
alter table 表名 drop index 列名;
# 错误格式
alter table 表名 modify 列名 数据类型;


# 主键约束: primary key
# 含义: 非空且唯一
# 一张表中只能有一个字段为主键
# 主键就是表中记录的唯一标识
# 在创建后添加主键约束
alter table 表名 modify 列名 数据类型 primary key; # 注意: 修改时确保数据中没有重复值 且 不为null
# 删除主键约束
# 正确格式
alter table 表名 drop primary key; # key后面没有列名, 因为主键就要一个。
# 错误格式
alter table 表名 modify 列名 数据类型;


# 自动增长: auto_increment
# 概念：
# 如果某一列是数值类型的, 
# 使用auto_increment 可以来完成值的自动增长。
create table 表名(
		列名 数据类型 primary key auto_increment
);
alter table 表名 id int auto_increment;
alter table stu modify id int;


# 外键约束: foreign key
# 使用级联操作
ON UPDATE CASCADE #级联更新
ON DELETE CASCADE #级联删除

alter table 表名1 add constraint 外键名
foreign key(外键列)
references 表名2(对应外键列)
no update cascade
no delete cascade;
# 两个可以同时添加

# 级联操作很危险 用的时候需要谨慎！
```





## 函数

字符串函数

```mysql
# 字符串拼接，将S1，S2,Sn拼接成一个字符串
select CONCAT('hello',',','word'); # hello,word

# 将字符串str全部转为小写
select LOWER('Hello,Word'); # hello,word

# 将字符串str全部转为大写
select UPPER('Hello,word'); # HELLO,WORD

# 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度
select LPAD('ABC',6,'---'); # ---ABC

# 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度
select RPAD('ADB',6,'---'); # ADB---

# 去掉字符串头部和尾部的空格
select TRIM(' Hello  word '); #Hello  word#

# 返回从字符串str从stat位置起的len个长度的字符串
select SUBSTRING('Hello,Word',1,5) # Hello 

# 企业员工的工号，统一为5位数，目前不足5位数的全部在前面补0
update emp set workno Lpad(workno,5,0')
```

数值函数

```mysql
# 向上取整
select CEIL(1.5); # 2
select CEIL(-1.5); # -1

#向下取整
select FLOOR(1.5); # 1
select FLOOR(-1.5); # -2

# 返回x/y的模
select MOD(3,2); # 1
select MOD(3,0); # null

# 生成0~1 之间的小数
select RAND(); # 0.6089404976078686

# 对x的四舍五入，y是保留小数的位数
select ROUND(1.356,3); # 1.356
select ROUND(1.356,2); # 1.36
select ROUND(1.356,1); # 1.4

# 生成一个6位数的验证码
select LPAD(ROUND(RAND()*1000000,0),6,'0'); # 由于是0~1之间的数 乘以倍数后可能不是6位数，要填充补位
# 当然
# ROUND 换成 CEIL / FLOOR
# LPAD  换成 RPAD 都可以
```



常见的日期函数如下：

| 函数                              | 功能                                              |
| :-------------------------------- | :------------------------------------------------ |
| CURDATE()                         | 返回当前日期                                      |
| CURTIME()                         | 返回当前时间                                      |
| NOW()                             | 返回当前日期和时间                                |
| YEAR(date）                       | 获取指定date的年份                                |
| MONTH(date)                       | 获取指定date的月份                                |
| DAY(date)                         | 获取指定date的日期                                |
| DATE ADD(date,INTERVAL expr type) | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
| DATEDIFF(date1,date2)             | 返回起始时间date1和结束时间date2之间的天数        |

















