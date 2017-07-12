---
title: MySQL语句
date: 2017-07-12 22:58:09
tags: mysql
---

# MySQL语句

@(MySQL)[数据库]

[TOC]

## 初涉MySQL

### MySQL的安装与配置
default-character-set=utf8
character-set-server=utf8

### MySQL常用命令以及语法规范
MySQL语句规范：
关键字与函数名称全部大写；
数据库名称、表名称、字段名称全部小写；
SQL语句必须以分号结尾；

修改提示符：
PROMPT

| 参数      |     描述|
| :-------- |  :------: |
| \D|     完整的日期|
| \d|     当前数据库|
| \h|     完整的服务器名称|
| \u|     当前用户|

### 数据类型
整型：

| Col1      |     存储范围|   字节|
| :-------- | :--------| :------: |
| tinyint |   有符号：-128到127 （-2的7次方到 2的7次方-1） 无符号 ：0-255 （0-2的8次方-1）|  1|
| smallint |   有符号：-128到127 （-2的7次方到 2的7次方-1）无符号 ：0-255 （0-2的8次方-1）|  2|
| mediumint |   有符号：-8388608到8388608 （-2的23次方到 2的23次方-1）无符号 ：0-16777215 （0-2的24次方-1）|  3|
| int |   有符号：-2147483648到2147483648 （-2的31次方到 2的31次方-1）无符号 ：0-4294967295 （0-2的32次方-1）|  4|
| bigint |   有符号：-9223372036854775808到9223372036854775808 （-2的63次方到 2的63次方-1）无符号 ：0-18446744073709551616 （0-2的63次方-1）|  8|
1.整形
1.1 tinyint 1字节
有符号：-128到127 （-2的7次方到 2的7次方-1）
无符号 ：0-255 （0-2的8次方-1）

1.2 smallint 2字节
有符号：-32768到32768 （-2的15次方到 2的15次方-1）
无符号 ：0-65535 （0-2的16次方-1）

mediumint 3字节
有符号：-8388608到8388608 （-2的23次方到 2的23次方-1）
无符号 ：0-16777215 （0-2的24次方-1）

int 4字节
有符号：-2147483648到2147483648 （-2的31次方到 2的31次方-1）
无符号 ：0-4294967295 （0-2的32次方-1）

bigint 8字节

有符号：-9223372036854775808到9223372036854775808 （-2的63次方到 2的63次方-1）
无符号 ：0-18446744073709551616 （0-2的63次方-1）

### 操作数据库
创建数据库：

	CREATE {DATABASE | SCHEMA}[IF NOT EXISTS] db_name [[DEFAULT] CHARACTER SET [=] charset_name];
	
修改数据库：

	ALTER {DATABASE|SCHEMA}[db_name] [DEFAULT] [CHARACTER SET [=] charset_name];
			
删除数据库：
	
	DROP {DATABASE|SCHEMA}[IF EXISTS] db_name;

查看数据库：
	
	SHOW DATABASE; 

警告信息：

	SHOW WARNINGS;
	
SHOW CREATE DATABASE t1;

##基础操作

### INSERT 插入
- INSERT INTO
``` sql
INSERT [INTO] table_name [(column_name,...)] {VALUES|VALUE} ({expr|DEFAULT},...),(...),...;
```
此方法比较常用，可以一次性插入多条记录，并且可以输入表达式甚至是函数，但是无法进行子查询。

- INSERT SET
``` sql
INSERT [INTO] tb1_name SET col_name={expr|DEFAULT},...;
```
与第一种方式的区别在于，此方法可以使用子查询（SubQuery）;且只能一次性插入一条记录。

- INSERT SELECT
``` sql
INSERT [INTO] table_name [(column_name,...)] SELECT...;
```
此方法可以将查询结果插入到指定数据表中

举例：
``` sql
INSERT test（username） SELECT username FROM users WHERE age >=30;
```

### UPDATE 更新（单表）
``` sql
UPDATE [LOW_PRIORITY] [IGNORE] table_reference SET col_name1={expr1|DEFAULT}[,col_name2={expr2|DEFAULT}]...[WHERE where_condition];
```
一般来说要用WHERE指定位置，不然所有数据都会被更新.

举例：
``` sql
UPDATE users SET age = age + 5 - id,sex = 0; 
```
//更新多个字段的值
``` sql
UPDATE users SET age = age+ 10 WHERE id % 2=0; 
```
//更新id为偶数的位置age的值

### DELETE 删除（单表）
``` sql
DELETE FROM tbl_name [WHERE where_conditon];
```

### SELECT 查找
``` sql
SELECT select_expr [,select_expr ...] --只查找某一个函数或表达式
[
FROM table_references --查询表名
[WHERE where_conditon] --查询条件
[GROUP BY {col_name|position} [ASC|DESC],...] --按某个字段进行分组，相同的只显示第一个
[HAVING where_conditon] --分组时，给出显示条件
[ORDER BY {col_name|expr|position} [ASC|DESC],...] --排序
[LIMIT {[offset,]row_count|row_count OFFSET offset}] --限制返回数量
]
```
每一个表达式表示想要的一列，必须至少有一个
多个列之间以英文逗号分隔
星号( * )表示所以列 tbl_name.*可以表示命名表的所有列
查询表达式可以使用[As]alias_name为其赋予别名
别名可用于GROUP BY，ORDRE BY或HAVING子句

SELECT 字段出现顺序影响结果集出现顺序，字段别名也影响结果集字段别名。
 
 - 利用GROUP BY 分组 添加分组条件 [HAVING where_condition]
要么为一个聚合函数，要么出现在SELECT 条件中。（聚合函数：max(),min(),avg(),sum(),count() 永远只有一个返回结果）

##子查询和连接

### 子查询简介
- 子查询是指出现在【其他SQL语句内】的SELECT子句
eg：
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2);
其中，SELECT * FROM t1 ...称为Outer Query（外查询或者Outer Statement）
SELECT column1 FROM t2 称为Sub Query（子查询）

- 子查询可以返回标量、一行、一列或者子查询。

### 使用比较运算符的子查询
- 子查询查询出的结果出现多个时，使用ANY\SOME（符合其中一个），ALL（全部符合）来来修饰。

### [NOT] IN 的子查询
=ANY 等价于 IN
!=ALL或<>ANY等价于NOT IN

### 使用[NOT]EXISTS的子查询（不常用）：
若子查询返回任何行，则返回TRUE，否则为FALSE。

### 使用INSERT...SELECT插入记录

	INSERT [INTO] tbl_name SET col_name={exprDEFAULT},... --可以使用子查询
	INSERT [INTO] tbl_name [(col_name,...)] SELECT ... --将查询结果写入数据表
	
eg：
DESC tdb_goods_cates; //显示出tdb_goods_cates表中的项目名称，与SHOW COLUMNS FROM tdb_goods_cates;作用相同

INSERT tdb_goods_cates(cate_name) SELECT goods_cate FROM tdb_goods GROUP BY goods_cate;//在表tdb_goods_cates中插入tdb_goods中的cate

### 多表更新

	UPDATE table_references SET col_name1={expr1|DEFAULT} [,col_name2={expr2|DEFAULT}]... [WHERE where_condition]
	
INNER JOIN,内连接
在MySQL中，JOIN, CROSS JOIN 和 INNER JOIN 是等价的。
LEFT [OUTER] JOIN ,左外连接
RIGHT [OUTER] JOIN,右外连接

	update tdb_goods inner join tdb_goods_cates on goods_cate=cate_name set goods_cate=cate_id;

### 多表更新（一步到位）
建表、查询、写入三合一

	CREATE TABLE tdb_goods_brands (
	brand_id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	brand_name VARCHAR(40) NOT NULL
	) SELECT brand_name FROM tdb_goods GROUP BY brand_name;

多表更新

	UPDATE tdb_goods INNER JOIN tdb_goods_brands ON tdb_goods.brand_name = tdb_goods_brands.brand_name
	SET tdb_goods.brand_name = tdb_goods_brands.brand_id;
	
通过ALTER TABLE语句修改数据表结构
ALTER TABLE tdb_goods
CHANGE goods_cate cate_id SMALLINT UNSIGNED NOT NULL,
CHANGE brand_name brand_id SMALLINT UNSIGNED NOT NULL;

外键，不一定是物理的外键，逻辑的外键也行，当然，物理外键更能保证数据的完整性和一致性

注意：数字类型的字段占用的空间更小，查询的效率也更高

### 连接语法结构
MySQL在SELECT语句、多表更新、多表删除语句中支持JOIN操作。
语法结构

	table_reference A
	{[INNER|CROSS] JOIN | {LEFT|RIGHT} [OUTER] JOIN}
	table_reference B
	ON condition_expr

数据表参照
table_reference
tbl_name [[AS] alias] table_subquery [AS] alias
数据表可以使用tbl_name AS alias_name 或 tbl_name alias_name赋予别名。
table_subquery可以作为子查询使用在FROM子句中，这样的子查询必须为其赋予别名。

### 内连接INNER JOIN
1、内连接：在MySQL中JOIN,INNER JOIN,CROSS JOIN是等价的
2、外连接：LEFT JOIN左外连接；RIGHT JOIN右外连接
3、连接条件：使用ON设定连接条件，也可以用WHERE代替
· ON：设定连接条件
· WHERE：进行结果集记录的过滤
4：内连接是返回左表及右表符合连接条件的记录
eg：

	SELECT * FROM tabA JOIN tabB ON tabA.name = tabB.name; --表示返回都含有的name值对应的字段

### 外连接OUTER JOIN
1、LEFT JOIN：显示左表全部和左右符合连接条件的记录
2、RIGHT JOIN：显示左右符合连接条件的记录和右表全部记录
3、若某字段只存在某一表，则另一表的里字段返回null

### 多表连接

	SELECT goods_id,goods_name,cate_name,brand_name,goods_price FROM tdb_goods AS g
	INNER JOIN tdb_goods_cates AS c ON g.cate_id = c.cate_id
	INNER JOIN tdb_goods_brands AS b ON g.brand_id = b.brand_id\G;
	
注意：INNER和INNER之间是没有逗号的

### 关于外连接的几点说明
外连接：
以左外连接为例：
A LEFT JOIN B join_condition
数据表B的结果集依赖于数据表A
数据表A的结果集根据左连接条件依赖所有数据表(B表除外)
左外连接条件决定如何检索数据表B(在没有指定WHERE条件的情况下)
如果数据表A的某条记录符合WHERE条件，但是在数据表B不存在符合连接条件的记录，将生成一个所有列为空的额外的B行
内连接：
使用内连接查找的记录在连接数据表中不存在，并且在WHERE子句中尝试一下操作：column_name IS NULL 。如果 column_name 被指定为 NOT NULL，MySQL将在找到符合连接着条件的记录后停止搜索更多的行（查找冲突）

### 无极限分类表设计
无限分类：即在同一张表中既有父类，又有子类。
通过在分类表中再增加多一个字段标识其属于哪一个父类的 ID 来实现。
可以通过对同一张数据表的自身连接来进行查询，需要对表标识别名。

查找父级对应的名称：

	select s.type_id ,s.type_name,p.type_name As parent_name from tdb_goods_types s left join tdb_goods_types p on s.parent_id=p.type_id;

查找子级对应的名称：

	select p.type_id ,p.type_name,s.type_name from tdb_goods_types p left join tdb_goods_types s on p.type_id=s.parent_id;

查找有多少子级：

	select p.type_id ,p.type_name,COUNT(s.type_name) child_count from tdb_goods_types p left join tdb_goods_types s on p.type_id=s.parent_id GROUP BY p.type_name ORDER BY p.type_id;

### 多表删除
（1）多表删除，将重复记录删除，保留ID号比较小的项
（2）查找重复记录

	SELECT goods_id,goods_name FROM tdb_goods GROUP BY goods_name HAVING count(goods_name) >= 2;
	
（3） 删除重复记录

	DELETE t1 FROM tdb_goods AS t1 LEFT JOIN (SELECT goods_id,goods_name FROM tdb_goods GROUP BY goods_name HAVING count(goods_name) >= 2 ) AS t2 ON t1.goods_name = t2.goods_name WHERE t1.goods_id > t2.goods_id;

## 运算符和函数

### 字符函数

| 函数名称 |    描述   |
| :-------- | :------: |
| CONCAT()  |  字符连接 |
| CONCAT_WS()  |  使用指定的分隔符进行字符连接 |
| FORMAT()  |  数字格式化 |
| LOWER()  |  转换成小写字母 |
| UPPER()  |  转换成大写字母 |
| LEFT()  |  获取左侧字符 |
| RIGHT()  |  获取右侧字符 |

### 数值运算符和函数

### 比较运算符和函数

### 日期时间函数

### 信息函数

### 聚合函数

### 加密函数



