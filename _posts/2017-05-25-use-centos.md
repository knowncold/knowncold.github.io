---
title: CentOS的使用
layout: page
category: wiki
---
CentOS 和 Ubuntu 用起来很多地方还是很不一样的。

## 软件包管理

```
$ sudo yum update
$ sudo yum install software
```

## 安装MySQL

> `CentOS 7`将MySQL从默认的程序列表中移除，用`mariadb`代替了。

```
# yum install mariadb-server mariadb
```

### 启动 mariadb

```
# systemctl start mariadb
```

### 正常使用 mariadb

```
# mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 5.5.52-MariaDB MariaDB Server

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.00 sec)

MariaDB [(none)]>
```
