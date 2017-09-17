---
title: Docker化的mssql-server
layout: page
category: wiki
---

> Docker是比虚拟机小的虚拟化容器技术，每个Docker镜像都有一个基本的特定的功能，比如配置好PHP环境的镜像、数据库系统。

### Docker基本命令

    docker ps -a    # 查看当前的所有容器


## sssql-server

### 在Mac或者Windows上，需要将Docker的内存调到4GB

### 拉取镜像

    docker pull microsoft/mssql-server-linux

### 运行容器镜像

    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d microsoft/mssql-server-linux

### 修改SA密码

    docker exec -it <Container ID> /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<Old Password>' -Q 'ALTER LOGIN SA WITH PASSWORD="<New Password>";'

### 容器内部连接数据库

    docker exec -it e69e056c702d "bash"
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourPassword>'

    >1 CREATE DATABASE TestDB
    >2 SELECT Name from sys.Databases
    >3 go

    >1 USE TestDB
    >2 CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT)
    >3 INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
    >4 go

    >1 QUIT

### 从容器外部连接数据库

    sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'

### 可视化工具连接数据库

以DataGrip为例

![](http://o73wiy9vn.bkt.clouddn.com/docker-sql-server.png)
