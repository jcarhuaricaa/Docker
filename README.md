# Docker

### 1. Commands to create image for Dockerfile which contain html files for personal blog.

```bash
docker build --tag centos_apache:v1 .
docker images
docker run -d -p 9090:80 --name centos-yescmd centos_apache:v1
docker ps
```

![image](https://user-images.githubusercontent.com/9786713/178168558-065c9b65-c88f-44ac-98da-072c8716ddb3.png)

### 2. Commands to create image for Dockerfile2 which contain html files.


```bash
docker rm -f centos-yescmd
docker build -t nginx_custom:v1 -f Dockerfile2 .
docker images
docker run -d -p 9090:80 --name nginx nginx_custom:v1
docker ps
docker ps --no-trunc
```
![image](https://user-images.githubusercontent.com/9786713/178357367-5d2fa570-4321-4ede-9012-6f7c79ff812f.png)

### 3. Commands to go inside to the container and see the logs.

```bash
docker exec -ti apache10 bash
docker logs apache10
```
### 4. Command to run a container from an image without CMD.
```bash
docker run -d -ti -p 9091:80 --name apache10 centos_apache:v2
docker exec -ti apache10 bash
apachectl -DFOREGROUND
apachectl -DBACKGROUND
ps aux
```
The container will be alive as long as the CMD process will be alive. If we go inside the container and kill the CMD process the container will died.
```bash
docker exec -ti apache10 bash
ps aux
pkill -9 httpd
```

### 5. Commands to create image for Dockerfile3.
```bash
docker build -t apache_with_code:v11 -f Dockerfile3 --build-arg user=alex --build-arg httpd_package=httpd .
docker run -d -ti -p 9091:80 --name apache11 apache_with_code:v11
docker ps
docker logs -f apache11
```
