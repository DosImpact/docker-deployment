- [Mac Mini - monolithic Dev \& Ops Env](#mac-mini---monolithic-dev--ops-env)
- [env setup](#env-setup)
- [status](#status)
- [features](#features)
  - [mysql](#mysql)
  - [postgreSQL](#postgresql)
  - [redis](#redis)
  - [sonar qube](#sonar-qube)
  - [Jenkins](#jenkins)
  - [Portainer](#portainer)
- [Note](#note)



# Mac Mini - monolithic Dev & Ops Env

/Users/dosimpact/docker-volume

# env setup

```
PORTAINER_VOLUME_DIR=/Users/dosimpact/Volume/portainer
REDIS_PASSWORD=dosimpact
```

# status

```
Portainer     9900
Jenkins       9901
redis_cache   8000
redis_session 8001

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


## sonar qube

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

# Note

jenkins  

  - 아키텍쳐 변경
      - uname -m // aarch64
          uname에 맞는 아키텍쳐 설치파일이 필요하다. 이를 docker file 의 env로 주고 싶다.
  - 초기 비밀번호 파일이 안만들어져서, docker lgos를 찔르니 보임.