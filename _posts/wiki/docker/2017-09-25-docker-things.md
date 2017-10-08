---
title: Docker笔记
layout: page
category: wiki
hidden: true
---
## 常用命令
```bash
docker start 02ee30fa0053
docker stop 02ee30fa0053
docker rm 02ee30fa0053
docker ps -a
docker exec -it 02ee30fa0053 bash
docker-compose run --service-ports some-db
```

发现之前用`docker run`会连不上redis和MySQL，但是用`docker-compose`之后就可以了，很是奇怪

## MySQL

### docker-compose
```yml
some-mysql:
  image: mysql
  environment:
    MYSQL_ROOT_PASSWORD: password
  ports:
    - "3306:3306"
```

### 从内部命令行连接
```bash
docker exec -it 02ee30fa0053 bash
mysql -u root -p
```

用DataGrip之前似乎需要先用命令行创建一个`database`再连接

```sql
create database test;
show databases;
```

### 参考
<a href="https://hub.docker.com/_/mysql/">https://hub.docker.com/_/mysql/</a>

## Redis

### docker-compose
```yml
some-redis:
    image: redis
    ports:
        - "6379:6379"
```

### 参考
<a href="https://hub.docker.com/_/redis/">https://hub.docker.com/_/redis/</a>
