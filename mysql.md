# mysql 数据库

* 什么是MySQL数据库呢

1. 看图

<img src="C:\Users\故事与酒\AppData\Roaming\Typora\typora-user-images\image-20220104202246170.png" alt="image-20220104202246170" style="zoom:80%;" />

数据库的本质就是对应这一个文件夹(数据库) 和一个文件(表)

## 数据库的储存的方式------ 表 

表顾名思义就是有列有行

在数据库里 列是需要指定类型

行就是一条记录

## 数据库的创建

```mysql
create database datebase_name charset utf8 collate utf8_bin engine ;
```



* 说明一下: utf8 是字符集  utf8_bin 是校对规则 还有一个是engine  是存储引擎 

```mysql
# 显示全部的数据库
show databases
-- 显示数据库创建的语句 
show create database database_name;# 这里会显示字符集(默认utf8) 校对的规则(默认是utf8_general_ci) 以及 存储引擎(也有默认还不是很清楚)
-- 删除数据库
drop database database_name;
```

## 备份数据库

* 要在dos窗口下执行不能在MySQL下执行
* 语句:  mysqldump -u root -phsp -B 数据库1 数据库2 > 文件名.sql
* 恢复数据库的话
* source 文件名.sql (这个又要在MySQL下执行了)

## 表的一系列操作

```mysql
-- 创建表
create table table_name (列名 数据类型,列名 数据类型) charset 字符集 collate 校对规则 engine 引擎 (默认 和 数据的保持一致);
-- 修改表
-- 添加一列
alter table table_name
add (列名 数据类型) ,(列名 数据类型);
--修改一列
alter table table_name
modify (要修改的列名 新的数据类型);
-- 删除列
alter table table_name
drop 列名;
-- 修改表名
rename table 表名 to  新表名;
-- 修改表的字符集
alter table table_name charset 字符集;
-- 修改列的名字
alter table table_name
column column_name 新名字 数据类型
-- 显示表的结构
desc table_name;
```

* **对表中数据的操作**

``` mysql
-- 添加
insert into table_name (列名1,列名2,列名3)#默认是全部
values (value1,value2,value3) # 要一一对应 (可以不按顺序但一定要对应)
-- 更新
update table_name
set column_name1 = value1, column_name2 = value2
where 过滤的条件 # 如果不加那么就是那一列全部改变
-- 删除
delete from table_name
where # 不加的话就是删除全部的记录 加上条件就只删除哪一个记录
-- 查询
select (distinct #这个的意思是去重查找) column1,column2 (* 代表全部的列)
        from table_name
        where 加一些条件
        order column_name desc(降序) esc(升序)
-- (列)的别名
select column_name as 别名 from table_name;
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

