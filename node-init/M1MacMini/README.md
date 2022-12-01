- [Mac Mini - monolithic Dev \& Ops Env](#mac-mini---monolithic-dev--ops-env)
- [env setup](#env-setup)
- [status](#status)
- [features](#features)
  - [mysql](#mysql)
  - [postgreSQL](#postgresql)
  - [redis](#redis)
  - [redis-commander](#redis-commander)
  - [Jenkins](#jenkins)
  - [Portainer](#portainer)
  - [WIP - sonarqube](#wip---sonarqube)
  - [WIP - prom](#wip---prom)
  - [WIP - cadviser](#wip---cadviser)
  - [WIP - grafana](#wip---grafana)
- [Note](#note)
  - [jenkins](#jenkins-1)
  - [mysql:8.0](#mysql80)



# Mac Mini - monolithic Dev & Ops Env

/Users/dosimpact/docker-volume

# env setup

```
PORTAINER_VOLUME_DIR=/Users/dosimpact/Volume/portainer
REDIS_PASSWORD=dosimpact
```

# status

```
Portainer         9900
Jenkins           9901

redis_cache       8000
redis_session     8001
redis-commander   8100

postgres          8500
mysql             8600
mongodb           8700
mongodb-admin     8701

```

# features


## mysql

```yml
version: "0.1.0"

services:
  mysql_db:
    image: mysql/mysql-server
    container_name: mysql_session
    restart: always
    volumes:
      - /volume_mysql:/var/lib/mysql
    ports:
      - "5000:3306"
    environment:
      - MYSQL_ROOT_PASSWORD=dosimpact
```

## postgreSQL

```yml

version: "0.1.0"

services:
  postgre_db:
    image: postgres:13
    container_name: algoridang_price
    restart: always
    volumes:
      - /algoridang_price:/var/lib/postgresql/data
    ports:
      - "6000:5432"
    environment:
      - POSTGRES_PASSWORD=dosimpact


```




## redis

- redis-cli -h dosimpact-2.iptime.org -p 8000 -a <PASSWORD>

```yml

version: "0.1.0"

services:
  redis_session:
    image: redis:latest
    container_name: redis_session
    restart: always
    ports:
      - "3000:6379"
    command:
      - /bin/sh
      - -c
      - redis-server --appendonly yes --requirepass ${REDIS_PASSWORD}
    # env_file: .env
```


## redis-commander

```
version: "0.1.0"

services:
  redis:
    image: redis:latest
    container_name: redis
    restart: always
    ports:
      - "6000:6379"
    command:
      - /bin/sh
      - -c
      - redis-server --appendonly yes --requirepass ${REDIS_PASSWORD}
    env_file: .env
  redis-commander:
    container_name: redis-commander
    hostname: redis-commander
    image: rediscommander/redis-commander:latest
    restart: always
    environment:
      - HTTP_USER=admin
      - HTTP_PASSWORD=${HTTP_PASSWORD}
      - REDIS_HOST=
      - REDIS_PORT=6000
      - REDIS_PASSWORD=${REDIS_PASSWORD}
    ports:
      - "8081:8081"
# docker-compose --env-file ./.env.dev up -d
# redis-cli -h 127.0.0.1 -p 5001 -a dosimpact

```

## Jenkins

goal : github -> auto docker build -> docker hub push 


```
FROM jenkins/jenkins:lts
USER root

RUN mkdir -p /tmp/download && \
 curl -L https://download.docker.com/linux/static/stable/aarch64/docker-18.03.1-ce.tgz | tar -xz -C /tmp/download && \
 rm -rf /tmp/download/docker/dockerd && \
 mv /tmp/download/docker/docker* /usr/local/bin/ && \
 rm -rf /tmp/download && \
 groupadd -g 999 docker && \
 usermod -aG staff,docker jenkins

RUN apt-get update
# only test
RUN apt-get -y install sudo

USER jenkins
```


```yml
version: "0.1.0"

services:

  jenkins-docker:
    build:
      context: .
      dockerfile: ./Dockerfile
    container_name: jenkins-docker
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /home/ubuntu/workspace/jenkins_home:/var/jenkins_home
    ports:
      - "8000:8080"
      - "50000:50000"
    user: root

---

```

## Portainer

goal : docker hub -> auto deployment  

```yml
version: "0.1.0"

services:
  portainer:
    image: portainer/portainer-ce
    container_name: portainer
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ${PORTAINER_VOLUME_DIR}:/data
    ports:
      - "9900:9000"
    env_file: .env
```


## WIP - sonarqube

## WIP - prom

## WIP - cadviser

## WIP - grafana



# Note

## jenkins  

  - 아키텍쳐 변경
      - uname -m // aarch64
          uname에 맞는 아키텍쳐 설치파일이 필요하다. 이를 docker file 의 env로 주고 싶다.
  - 초기 비밀번호 파일이 안만들어져서, docker lgos를 찔르니 보임.


## mysql:8.0

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
