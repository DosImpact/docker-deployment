# Jenkins DSL 정의

# jenkins dockerfile   


```
docker build -t jenkins-docker:0.2 .

docker run -itd \
  -p 50001:50000 \
  -p 8000:8080 \
  -v /Users/dos/DockerVolumns/jenkins_home_v2:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -u root \
  --name jenkins_v2 \
  --restart always  jenkins-docker:0.2

docker exec -it jenkins bash
docker ps
```

```
docker run -itd \
  -p 50000:50000 \
  -p 8000:8080 \
  -v /home/ubuntu/workspace/volumns/jenkins:/var/jenkins_home \
  --name jenkins \
  --restart always  jenkins/jenkins:lts
```