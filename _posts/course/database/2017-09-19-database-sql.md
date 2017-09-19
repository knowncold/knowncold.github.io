---
title: 数据库与msSQL
layout: page
category: wiki
---

## 概念

把语句看成几个clause的组合，每个子句的输出是下一个子句的输入

DDL只有`alter`,`drop`,`create`

select操作的对象类型是表，relation

## SELECT

select + 列名 + 字符串

```sql
select
from
where
group by
having
order by
```

### select相关的运算顺序

1. from
2. where
3. group by
4. having
5. select
6. order by

### GROUP BY

GROUP BY的输出结果，一行是之前很多行的一组，一般会违反原子性

```sql
SELECT count(*), DEPTNO     -- 需要保证只出现一个值，无论是否有人为的控制，必须放到group子句去或者从语句上直接避免原子性
FROM EMP
GROUP BY DEPTNO
```

## 内建函数
```sql
isnull(var, 0)
d between 1 and 2 --两边闭区间
```

## 逻辑表达式
SQL没有`==`，没有连续的比较运算符写法`12<c<45`

使用`not`时，需要括号

```sql
where 1=2 AND a is not NULL AND b is NULL
```

## 模糊匹配

字符串内部也不区分大小写
```sql
where ename='JonEs'
where ENAME like 'j%'   -- %匹配任意长度任意字符
where ENAME like '%s'
where ENAME like '_o%'  -- _匹配一个任意字符
where ENAME like 'M_%' ESCAPE 'M' -- 任意字符都可以作为转义符号
```

### 正则表达式

待补充
