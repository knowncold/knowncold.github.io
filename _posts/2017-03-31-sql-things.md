---
title: SQL笔记
layout: wiki_page
category: wiki
---
### 用处

	SQL 面向数据库执行查询
	SQL 可从数据库取回数据
	SQL 可在数据库中插入新的记录
	SQL 可更新数据库中的数据
	SQL 可从数据库删除记录
	SQL 可创建新数据库
	SQL 可在数据库中创建新表
	SQL 可在数据库中创建存储过程
	SQL 可在数据库中创建视图
	SQL 可以设置表、存储过程和视图的权限

### 语法
分为`DML`和`DDL`两个，大小写不敏感 
SQL使用单引号来环绕文本值

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

#### SELECT
从表中选取列

```sql
SELECT RowName FROM TableName
SELECT LastName FROM Persons
SELECT FirstName, LastNmae FROM Persons
SELECT DISTINCT Company FROM Orders		/*从表中选取唯一不同的值*/
SELECT 'hello world!' AS Greeting		/*没有表也可以*/
```

#### WHERE

操作符

	=	等于
	<>	不等于
	>	大于
	<	小于
	>=	大于等于
	<=	小于等于
	BETWEEN	在某个范围内
	LIKE	搜索某种模式

```sql
SELECT RowName FROM TableName WHERE RowName = Value
SELECT * FROM Persons WHERE City='Beijing'
select * from students WHERE IsActive=1;
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





