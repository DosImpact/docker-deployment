# Welcome PI - DB Server

## 5000

- algoridang_db

```md
docker run -d --name algoridang_db \
-e POSTGRES_PASSWORD=dosimpact \
-v /algoridang_db:/var/lib/postgresql/data \
-p 5000:5432 \
--restart always postgres:13
```

## 6000

- algoridang_price

```md
docker run -d --name algoridang_price \
-e POSTGRES_PASSWORD=dosimpact \
-v /algoridang_price:/var/lib/postgresql/data \
-p 6000:5432 \
--restart always postgres:13
```

## 7000

- node_db (mysql:8.0)

```
sudo mkdir node_db
docker pull mysql/mysql-server

docker run -d \
    -p 7000:3306 \
    -e MYSQL_ROOT_PASSWORD=dosimpact \
    -v /home/ubuntu/workspace/node_db:/var/lib/mysql \
    --name node_db \
    --restart always mysql/mysql-server

docker exec -it node_db mysql -uroot -p

show databases;

// mysql은 localhost 만 허용을 한다.
create user 'root'@'221.153.254.18' identified by 'dosimpact';
create user 'root'@'192.168.0.1' identified by 'dosimpact';
flush privileges;

// 결과

use mysql;
select host, user from user;
+----------------+------------------+
| host           | user             |
+----------------+------------------+
| 192.168.0.1    | root             |
| 221.153.254.18 | root             |
| localhost      | healthchecker    |
| localhost      | mysql.infoschema |
| localhost      | mysql.session    |
| localhost      | mysql.sys        |
| localhost      | root             |
+----------------+------------------+
7 rows in set (0.00 sec)

--- version

mysql> SELECT VERSION();
+-----------+
| VERSION() |
+-----------+
| 8.0.28    |
+-----------+

docker pull mysql/mysql-server:5.7
```

docker run -d \
 -p 8000:3306 \
 -e MYSQL_ROOT_PASSWORD=dosimpact \
 -v /home/ubuntu/workspace/node_db_5:/var/lib/mysql \
 --name node_db_5 \
 --restart always mysql/mysql-server:5.7
