---
title: SQL笔记
layout: page
category: wiki
---

## 概念
* 数据库是保存有组织的数据的容器，经常和DBMS被混淆。
* 表是某种特定类型数据的结构化清单，同一数据库里面不能有同样的表名。
* 关于数据库和表的布局和特性的信息就是模式。
* 列是表中的一个字段。所有表都是由一个或多个列组成的。
* 数据类型是所允许的数据的类型。每个表列都有相应的数据类型，它限制（或允许）该列中存储的数据。
* 行是表中的一个记录。
* 主键是一列（或一组列），其值能够唯一标识表中每一行。
* SQL语句由子句构成，有些子句是必需的，有些则是可选的。一个子句通常由一个关键字加上所提供的数据组成。
* NULL无值（no value） ，它与字段包含 0、空字符串或仅仅包含空格不同。
* 操作符（operator）用来联结或改变 WHERE 子句中的子句的关键字，也称为逻辑操作符（logical operator）。

## 常见的习惯
SQL也有ANSI SQL

### 分解数据
按需求分解到一定程度，而不是极端的不可分。

### 数据类型的兼容
虽然大多数基本数据类型得到了一致的支持，但许多高级的数据类型却没有。更糟的是，偶然会有相同的数据类型在不同的DBMS中具有不同的名称。

### 主键的条件
* 任意两行都不具有相同的主键值
* 每一行都必须具有一个主键值（主键列不允许NULL值）
* 主键列中的值不允许修改或更新
* 主键值不能重用（如果某行从表中删除，它的主键不能赋给以后的新行）

## 语法
语句不区分大小写，一般列名和表名小写，SQL大写
分为`DML`和`DDL`两个
SQL使用单引号来环绕字符串

	DML
	SELECT - 从数据库表中获取数据
	UPDATE - 更新数据库表中的数据
	DELETE - 从数据库表中删除数据
	INSERT INTO - 向数据库表中插入数据

	DDL
	CREATE DATABASE - 创建新数据库
	ALTER DATABASE - 修改数据库
	CREATE TABLE - 创建新表
	ALTER TABLE - 变更（改变）数据库表
	DROP TABLE - 删除表
	CREATE INDEX - 创建索引（搜索键）
	DROP INDEX - 删除索引

### SELECT
从表中选取列

```sql
SELECT RowName FROM TableName		-- 单列查询
SELECT FirstName, LastNmae FROM Persons	-- 多列查询
SELECT Row AS NewName FROM TableName		-- 给检索出来的列新的名字,AS可以省略
SELECT * FROM TableName
SELECT DISTINCT Company FROM Orders		/*从表中选取唯一不同的值*/
SELECT 'hello world!' AS Greeting		/*没有表也可以*/
```

#### DINSTINCT
作用于后面的所有列

```
SELECT DISTINCT vend_id, prod_price
FROM Products
```

#### 限定数量
在SQL Server和Access中

```sql
SELECT TOP 5 prod_name
FROM Products;
```

DB2

```sql
SELECT prod_name
FROM Products
FETCH FIRST 5 ROWS ONLY;
```

Oracle

```sql
SELECT prod_name
FROM Products
WHERE ROWNUM <= 5;
```

MySQL，MariaDB，PostgreSQL，SQLite

```sql
SELECT prod_name
FROM Products
LIMIT 5;

-- 检索偏移
SELECT prod_name
FROM Products
LIMIT 5 OFFSET 5;

-- MySQL，MariaDB，SQLite 支持简写
SELECT prod_name
FROM Products
LIMIT 3, 4;
```

#### 第0行
检索出来的第一个是第0行，`LIMIT 1 OFFSET 1`会检索第二行而不是第一行

### ORDER BY
保证它是 SELECT 语句中最后一条子句。

```sql
SELECT prod_name
FROM Products
ORDER BY prod_name;
```

不一定用检索出来的列排序，也可以用非检索列排序。

#### 多列排序
按照列名

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price, prod_name;
```

按照相对列的位置

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY 2, 3;
```

#### 排序方向
默认是升序排序，降序排序`DESC`只作用于它前面的列名，具体的排序规则由数据库的属性决定

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC, prod_name;
```

### WHERE数据过滤
过滤数据应该在数据库端完成，减少不必要的客户端应用的工作和带宽的浪费。

#### 操作符

```
=	等于
<>	不等于
!=	不等于
>	大于
!>	不大于
<	小于
!<	不小于
>=	大于等于
<=	小于等于
BETWEEN 5 AND 10	-- 在某个范围内，闭区间
LIKE	搜索某种模式
IS NULL	为NULL值
```

```sql
SELECT RowName
FROM TableName
WHERE RowName = 12;

SELECT *
FROM Persons
WHERE City='Beijing';

SELECT *
FROM students
WHERE credit > 10;
```

不等于某个值，又包含NULL值

```sql
SELECT *
FROM Customers
WHERE cust_email <> 'dstephens@fun4all.com' OR cust_email IS NULL;
```

#### 逻辑操作符

`AND`优先级高于`OR`，可以使用圆括号

```sql
AND
OR		-- 有些DBMS中OR第一个条件满足就不计算第二个了
IN ('a', 'b')	-- 由逗号分隔、括在圆括号中的合法值	相当于OR和=，但速度更快，而且在IN里面可以加子句
NOT		-- 有些DBMS支持NOT与IN、BETWEEN、EXISTS连用
```

#### 通配符过滤
不要把它们用在搜索模式的开始处。把通配符置于开始处，搜索起来是最慢的。

```sql
SELECT prod_id, prod_name  
FROM Products  
WHERE prod_name LIKE 'Fish%';
```

`%`匹配任意0个、一个、多个字符：`'F%y'`，不会匹配`NULL`

> 注意后面的空格，包括 Access在内的许多 DBMS都用空格来填补字段的内容。 例如， 如果某列有 50个字符，而存储的文本为 Fish bean bag toy（17个字符） ，则为填满该列需要在文本后附加 33 个空格。这样做一般对数据及其使用没有影响， 但是可能对上述SQL语句有负面影响。 子句WHERE prod_name LIKE 'F%y'只匹配以 F 开头、以 y 结尾的 prod_name。폠怀如果值后面跟空格，则不是以 y 结尾，所以 Fish bean bag toy 就不会检索出来。 简单的解决办法是给搜索模式再增加一个%号： 'F%y%'

> Microsoft Access，需要使用*而不是%
> 看DBMS的配置，有些区分大小写

`_`匹配单个字符

>  Microsoft Access，需要使用?而不是_;DB2不支持_

[]指定字符集，匹配字符集中的一个字符，只有Access和SQL Server支持，使用`[^JM]`来否定

```sql
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[JM]%'
ORDER BY cust_contact;
```

### 计算字段

#### 拼接字段

> Access和 SQL Server使用+号。DB2、Oracle、PostgreSQL、SQLite和Open Office Base 使用||。
> 在 MySQL和 MariaDB中，必须使用特殊的函数。

```sql
SELECT vend_name + ' (' + vend_country + ')'
FROM Vendors
ORDER BY vend_name;
```

```sql
SELECT Concat(vend_name, ' (', vend_country, ')')
FROM Vendors
ORDER BY vend_name;
```

很多数据库返回的时候会把这个新列保存成列宽的文本值，导致后面会有空格，通过`RTRIM`、`LTRIM`、`TRIM`去掉空格

```sql
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')'
FROM Vendors
ORDER BY vend_name;
```

#### 算术计算
支持`+、-、*、／`

```sql
SELECT prod_id, quantity, item_price, quantity*item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008;
```

### 函数

#### 文本处理

----|----
LEFT()（或使用子字符串函数） |返回字符串左边的字符
LENGTH()（也使用DATALENGTH()或LEN()） |返回字符串的长度
LOWER()（Access使用LCASE()） |将字符串转换为小写
LTRIM() |去掉字符串左边的空格
RIGHT()（或使用子字符串函数） |返回字符串右边的字符
RTRIM() |去掉字符串右边的空格
SOUNDEX() |返回字符串的SOUNDEX值
UPPER()（Access使用UCASE()） |将字符串转换为大写

SOUNDEX()通过发音类似来匹配

```sql
SELECT cust_name, cust_contact
FROM Customers
WHERE SOUNDEX(cust_contact) = SOUNDEX('Michael Green');
```

#### 日期和时间处理函数
各种DBMS都很不一样

```sql
--- SQL Server
SELECT order_num
FROM Orders
WHERE DATEPART(yy, order_date) = 2012;
```

#### 数值处理

----|----
ABS() |返回一个数的绝对值
COS() |返回一个角度的余弦
EXP() |返回一个数的指数值
PI() |返回圆周率
SIN() |返回一个角度的正弦
SQRT() |返回一个数的平方根
TAN() |返回一个角度的正切

### 汇总数据

#### 聚集函数
对某些行运行的函数，计算并返回一个值。

----|----
AVG() |返回某列的平均值
COUNT() |返回某列的行数
MAX() |返回某列的最大值
MIN() |返回某列的最小值
SUM() |返回某列值之和

`AVG()`忽略`NULL`的行。

如果指定列名，则 COUNT()函数会忽略指定列的值为空的行，但如果COUNT()函数中用的是星号（\*） ，则不忽略。

虽然 MAX()一般用来找出最大的数值或日期值，但许多（并非所有） DBMS 允许将它用来返回任意列中的最大值，包括返回文本列中的最大值。在用于文本数据时，MAX()返回按该列排序后的最后一行，忽略列值为`NULL`的行；`MIX()`正好相反。

```sql
SELECT SUM(item_price*quantity) AS total_price
FROM OrderItems
WHERE order_num = 20005;
```

`SUM()`忽略`NULL`。

### JOIN

```sql
SELECT products.*, companies.name AS company_name
FROM products
JOIN companies
ON company_id = companies.id
```
