---
title: 数据库系统概念
layout: page
category: wiki
---

数据库系统是数据加上用于访问的一系列程序。

## What is DATA in a database

本身任何东西都能作为数据，但是需要另一种数据来说明这个东西，两种数据放在一起才能成为有意义的信息。  
比如不同格式的图像数据，需要指明读取的格式才能成为有意义的图片。

## 为什么不用文件储存来当作数据库

- Data redundancy and inconsistency
数据冗余是可行的，但是可能会出现一处数据改动另一处没有改动，导致数据不一致。

- Difficulty in accessing data
对于不同的检索条件，需要编写完全不同的新的程序

- Data isolation
数据可能分散在不同位置、不同格式的文件中，检索的时候就会非常麻烦

- Integrity problems
数据需要有一定的约束条件，但是有新的约束出现时，很难修改代码来保证新的约束的实现

- Atomicity problems
数据改动的操作必须是原子性的，不可再分的

- Concurrent-access anomalies
多个用户同时访问更新相关数据时，可能会出现两个同时修改的操作单个是对的，但是两个一起操作时只能一个生效导致错误发送

- Security problems
文件驱动的系统是ad hoc方式，无法实现不同的人对于不同的数据库拥有不同的访问权限

## Data Models

- data
- data relationships
- data semantics
- consistency constraints

- Conceptual Data Model
  * ER
- Logical Data Model
  * Relational
  * network
  * hierarchical
  * object-based
  * semistructured
- Physical Data Model
  * B* tree model

## Relational Model

relationships = row = n-tuple -> table -> database

笛卡尔积

属性的值是域，一个属性就是域的笛卡尔积的一个子集

- 有限集合
- 同一个关系的无序性
- 不同的元组的无序性
- 不同的属性可能会有同样的域

- 域必须是原子的（从实用的角度看，而不是数据本身）

#### null

null是unknown，属于所有的域

### 模式和实例

关系和数据库

### Key

- superkey
可以用来标识区分每个关系的属性的集合

- candidate key
最小的superkey

- primary key
人为选择的candidate key

- prime attributes
在candidate key中出现的属性

- non-prime attribute
没有在任何candidate key中出现的属性

#### foreign key

一个属性对应到一个主键，主键是外码，同一个数据表也行

### 其他

- 关系是元组的集合
- 外码不能唯一标识元组
- 码定义在关系上，而不是关系实例和模式
- 对于所有可能的实例都成立
