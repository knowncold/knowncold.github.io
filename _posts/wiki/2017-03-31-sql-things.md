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

#### WHERE
过滤数据应该在数据库端完成，减少不必要的客户端应用的工作和带宽的浪费。

操作符

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

#### JOIN

```sql
SELECT products.*, companies.name AS company_name
FROM products
JOIN companies
ON company_id = companies.id
```

#### ORDER

```sql
select * from people
where age > 50
order by age DESC;	/*DESC 逆序，ASC顺序*/
```
