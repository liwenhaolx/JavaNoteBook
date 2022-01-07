# mysql 数据库

* 什么是MySQL数据库呢

1. 看图

<img src="C:\Users\故事与酒\AppData\Roaming\Typora\typora-user-images\image-20220104202246170.png" alt="image-20220104202246170" style="zoom:80%;" />

数据库的本质就是对应这一个文件夹(数据库) 和一个文件(表)

## 数据库的储存的方式------ 表 

表顾名思义就是有列有行  这里的列可以看做是一个字段 一个变量

在数据库里 列是需要指定类型 还可以加以约束

行就是一条记录

## 数据库的创建

```mysql
create database datebase_name charset utf8 collate utf8_bin engine ;
```



* 说明一下: utf8 是字符集  utf8_bin 是校对规则 还有一个是engine  是存储引擎 // 默认是utf8  校对规则是 utf8_general_ci

```mysql
# 显示全部的数据库
show databases
-- 显示数据库创建的语句 
show create database database_name;# 这里会显示字符集(默认utf8) 校对的规则(默认是utf8_general_ci) 以及 存储引擎(也有默认还不是很清楚)
-- 删除数据库
drop database database_name;
```

## 备份数据库

* 要在dos窗口下执行**不能在MySQL下执行**
* 语句:  mysqldump -u root -phsp -B 数据库1 数据库2 > 文件名.sql
* 恢复数据库的话
* source 文件名.sql (这个**又要在MySQL下执行了**)

## 表的一系列操作

```mysql
-- 创建表
create table table_name (列名 数据类型,列名 数据类型) charset 字符集 collate 校对规则 engine 引擎 (默认 和 数据的保持一致);


-- 修改表
-- 添加列
alter table table_name
add (列名 数据类型) ,(列名 数据类型); # 就相当于增加一个变量集合
-- 修改列的数据类型
alter table table_name
modify (要修改的列名 新的数据类型); # 修改的只是这个列的数据类型
-- 删除列
alter table table_name
drop 列名;   # 是直接把这一列干掉
-- 修改表名
rename table 旧表名 to  新表名;
-- 修改表的字符集
alter table table_name charset 字符集;
-- 修改列的名字
alter table table_name
change column column_name(旧列名) 新名字 数据类型(数据类型还是要指定不指定不行)
-- 显示表的结构
desc table_name;
```

* **对表中数据的操作**

``` mysql
-- 添加
insert into table_name (列名1,列名2,列名3)#默认是全部
values (value1,value2,value3) # 要一一对应 (可以不按顺序但一定要对应)
-- 复制
insert into table_name
select * from table_name2
-- 更新
update table_name
set column_name1 = value1, column_name2 = value2
where 过滤的条件 # 如果不加那么就是那一列全部改变
-- 删除
delete 列名 from table_name  -- 注意一下 这里删除全行的记录是什么都不加就是删除全行记录 而不是 *
where # 不加的话就是删除全部的记录 加上条件就只删除哪一个记录
-- 查询
select (distinct #这个的意思是去重查找) column1,column2 (* 代表全部的列)
        from table_name
        where 加一些条件
        order column_name desc(降序) asc(升序)
-- (列)的别名
select column_name as 别名 from table_name;
-- 表的别名就直接
   table_name table_name01 //新的别名
```









## 常用的列(字段)数据类型

| 名字         | 空间大小         | 说明                                                         |
| ------------ | ---------------- | :----------------------------------------------------------- |
| bit()        | 可以在()指定大小 | 位类型,指定的大小默认是1 范围是1~64  存储的方式是二进制写入的 |
| int家族      | 1~8 字节不等     | 可以指定不带符号unsigned                                     |
| float        | 4                | 单精度                                                       |
| double       | 8                | 双精度                                                       |
| decimal(m,d) | 可以指定大小     | m为总长度 d 为小数点的位数                                   |
| char()       | 最大255          | 里面是字符数他是固定的                                       |
| varchar()    | 字节数是65535    | 根据不同的字符集 字符数也会不同 一般会留出1~3个字节数用来标志大小 可变的 |
| text         |                  | 文本类型                                                     |
| longtext     |                  | 空间大一点的文本类型                                         |
| blob         |                  | 二进制                                                       |
| longblod     |                  | 空间大一点的二进制                                           |
| date         | 日期             | 只有年月日                                                   |
| datetime     |                  | 有年月日时分秒                                               |
| time         |                  | 只有时分秒                                                   |
| timestamp    | 时间戳           | 可以自动记录时间                                             |
| enum         |                  | 枚举类型 只能在有限的类型中选择                              |

## mysql 中的函数

* 函数的返回值会被当做一个新的列返回 要显示这个列 就要在**select** 的后面 因此调用函数就是在select的后面 

```mysql
-- 合计和统计函数 用来统计个数的
select count(*) -- * 代表全部的行记录 
select count(column_name) from table_name
where # 筛选的条件   -- 代表着一列的非空记录 最主要就是差在这里是非空的

-- sum() 用来进行数据加和的 一般用于数值列 
select sum(column_name) from table_name
where -- 一些筛选的条件

-- avg() 用来求平均值地方函数
select avg(column_name) from table_name
where -- 筛选的条件

-- max / min  返回where 筛选条件下的最大 和 最小值
select max(column_name) from table_name
where   -- 筛选的条件
-- group by 来进行分组
select column1 , column2 , column3.. from table_name  -- 这里的column1 , column2,column3 是要在屏幕上显示的列
group by column  -- 这个column 是根据这个列来分组 就是将上面那些列来进行分组
-- 使用 having 来进行分组的过滤
select column1 , column2 , column3.. from table_name
group by column having -- 过滤条件
# 注意 用了group by  就要用 having来充当过滤的条件 而不是 使用 where



# 字符串相关的函数
-- charset() ---- 返回这个表某一字符列的字符集
select charset(column_name) from table_name;
-- concat() --- 连接字符串 可以将多个列拼接成一列
select concat(str1,str2,str3) from table_name;
-- instr --- 返回子字符串 在 字符串中出现的位置 没有就返回0  这里说明一下 在MySQL 就是以1为开头了
select instr(string,substring) from dual # 这里说明一下 这个dual 是 系统的表 可以作为测试表
-- ucase # 转换成大写
-- lcase # 转换成小写
-- left(string1,length) // 从string1 的左边取length个字符
-- right(string1, length) //从string1 的右边取length个字符
-- length(string) //返回他的长度
-- replace (str,str1,str2) // 在str(其实是一个字符变量集合(看末尾的理解)) 用str2 替换 str1 
-- strcmp(str1,str2) //比较大小
-- substring(str,position,length) // 在str中从position的位置开始 截取 length个字符
-- ltrim / rtrim / trim 都是拿来去空格的 l 代表左 r 代表右
# 说明我理解的字符变量集合 
-- 我认为 列名就是一个变量的集合的名字 他有类型 当他是字符类型的时候就是字符类型的集合  当然不一定对


# 数学相关的函数
-- abs() 绝对值
-- bin()  十进制 转 二进制
-- ceiling() 向上取整
-- conv() 进制转换
-- floor() 向下取整
-- format(num,decimal) // 保留decimal位小数 采取的是四舍五入
-- hex() 转16进制
-- least(num1,num2,num3) // 找最小的值
-- mod(num1 ,num2) //求余 num1 % num2
-- rand(seed) //随机数 当加入seed后随机数就固定了




# 日期函数
-- current_date() 当前日期
-- current_time() 当前时间
-- current_timestamp() 当前时间戳
-- date_add(time, time1)  // 以time为基准加上time1这么多的时间 !!! 注意!!! time1 可以是 年月日时分秒
-- date_sub()//和add 一样
-- datediff(time1,time2)  //求时间差  以天来返回
-- timediff()  //其他和datediff 一样 但是是按照时分秒来返回的
-- year() | date() | ....  在括号里可以是任意的时间类型 但是返回会截取相应的一段时间 比如 year() 只会要年的那一部分
-- unix_timestamp()  返回从1970-1-1 到现在的秒数 
-- from_unixtime()  将时间戳变成指定格式的日期  %Y-%m-%d %H:%i:%s  占位符的格式是指定的

-- !!! 注意可以用 数据库名.表名来指定表!!!




# 加密函数和系统的函数
-- user()  查看登录的用户是谁 是正在使用的用户
-- database() 查看数据库的名称是正在操作的数据库
-- md5() //32为加密字符串
-- password() // 系统的加密字符串

# 流程控制函数
-- if(expr1, expr2,expr3) //如果express1 是真 返回expr2 否则返回 expr3
-- ifnull(expr1,expr2) // 如果 expr1 是空就返回expr2 否则就返回expr1
-- 多分支
-- select case when expr1 then expr2
-- when expr3 then expr4
-- when expr5 then expr6
-- else 
-- end 
# when 后面的是真返回对应的then 后面的 注意 else select end 缺一不可

# !!!! 补充一下 !!!
-- 1. 时期类可以比较大小
-- 2. like 模糊查询 % : 表示是0~多个字符   _ : 代表一个字符 
-- 3. order by 是支持多个条件排序的  是按照 条件出现的先后顺序来决定优先级的 
-- 4.select * from table_name 
-- 5.order by sal,time   //先按照sal排序 在这个基础上在按照 time来排序  (desc)-- 降序 (asc) -- 升序(默认)
-- 6. 对于数据的判断要用 = 而对于null的判断要用 is
-- 8. 逻辑运算是 and -- && , or --- ||  ,not --- !
```

## 函数应用进阶

```mysql
-- 这里开始讲讲函数的综合应用


-- 函数的分页查询
# 就是多了一个limit字句
-- select ...
-- limit start,rows  # 解释一下 start 的意思是从start 开始 但是不包括start 的rows行记录

-- 讲讲select语句的子语句的顺序
-- 先分组 -- 过滤--- 排序---分页
-- select column from table_name
-- group by .. 
-- having ..
-- order by ..
-- limit start rows
# 这里似乎where和having不能共存 我不是很清楚 不一定是对的

-- 看看多表查询
-- 在没有限制的情况下,多表查询会出现笛卡尔集的问题 什么是笛卡尔集呢
select column1, column2 from table1 ,table2 
/* 出现两张表的时候 table1 的每一个元素 和 table的全部元素都要组合一次  */
/* 还要注意的是 若不出现笛卡尔集的问题 那么限制的条件应该是 总表数 - 1 */

-- 自连接
/*什么是自连接呢 就是一张表当两张表用  但是不同的是 这样就必须把 表名命名成别名 因为 mysql 对于同名的表没有办法区别
   然后就是 表名的别名 和 列名不同 是不需要as 的  格式:  表名 别名 */
   
-- 子查询 说白了就是查询里面有查询 

# 单行子查询  就是返回的数据只有一行 一行用 =
# 多行子查询 就是返回的数据有多行 多行判断是否数据在这多行里 用 in
# 子查询可以看做一张临时的表

-- 关键字 all and any  all : 就是全部都要满足  any : 满足其一就可以 (这两个关键字用于 多行的子查询)

-- 多列查询 这个多列查询是可以用多表查询 代替的 只是在一些情况下 跟简单 尤其是判断相等的时候 
-- (字段1,字段2) = (select 字段1,字段2 from table_name )  ---( 这是一张临时表 )
```



## 表的自我复制

```mysql
# 表的复制就是将一张表全部的值给他

insert into table_name01
select field01 field02.. from table_name02

# 自我复制就是
insert into table_name01
select * from table_name01
```

## 合并查询

```MySQL
# 就是将两个查询语句合并在一起
-- 用关键字 union -- 去重 (当两个查询语句返回的内容是有相同的时候要去重)
-- union all -- 不去重


```



## 外连接

* 分为左外连接 和 右外连接
* 左外连接是指 在两张表匹配的时候就算左边的表没有匹配到的也要显示出来 即 左边的全部都要显示
* 右外连接 是一样的就不多说了

```MySQL
select .... from table1 left join table2 on ....(限制条件)  --- 这里的on 就是想当于以前的where
```

## 约束

约束就是对数据进行一定的限制



```mysql
-- 1. 主键  在定义列的后面加上 primary key
name varchar(32) primary key
-- 主键是不能重复而且不能为空 还有一张表只能有一个主键 但是可以有复合主键 就是多个列来组成一个主键   格式 : primary key (name,id)

-- 2. 非空
not null -- 就是非空

-- 3. 唯一(unique) 这个约束是能限制具体的数不能重复 但是null可以重复 可以有多个unique在一张表里

-- 4. foreign key 外键
-- 主要是用来定义主表和从表之间的关系  外键定义在从表上 而 外键指向主表的字段必须是主键约束 或者 是 unique约束 而且类型要相同 长度可以不同 表的引擎
-- 必须是 innodb
foreign key (id) references table(id)
-- 5. check() 在MySQL5.7 是没有用的,他的意思是限制 字段的范围
check(id in (1,2,3)) # 意思是 id 只能是 1 或 2或 3

-- 6.enum()虽然check()没用但是数据类型enum是有用的 
sex enum('男','女')  -- 意思是只能在男女之间选

-- 7.自增长
auto_increment
id int auto_increment -- 就是这个字段你不给他值就是让他从1开始在表中添加一个数据他自动加1 (可以给他传数也可以是null  null就是按照系统来) 但是传了数之后 会在表中找最大的数加1后
-- 作为 下一次的id 
# 自增长 要和 primary key  或者 unique 来配合这用
```

