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

### 6. Context is the directory where are located all the files or the dependencies defined on the Dockerfile. So we can modified the previus command as following command.
```bash
docker build -t apache_with_code:v11 -f docker-images/Dockerfile3 --build-arg user=alex --build-arg httpd_package=httpd docker-images/
```
and if you see the message "Sending build context to Docker daemon x.xx MB" the value x.xx MB will match with the size of the directory docker-images
![image](https://user-images.githubusercontent.com/9786713/178852167-2a5677c1-4136-4c5f-ac8e-b52bc2b31322.png)

![image](https://user-images.githubusercontent.com/9786713/178852057-1dee2b63-32d8-4fef-8982-266b8518a313.png)

the context file should be as small as possible. So If we need to ignore some folder or files we just have to create the file .dockerignore with the list of files or folders that we want to ignore. Docker automatically will read that file and ignore all the content inside that file.

<img width="315" alt="Screen Shot 2022-07-13 at 10 43 52 PM" src="https://user-images.githubusercontent.com/9786713/178893483-8dc336c5-f1e1-46eb-b9a6-030b9b49c28a.png">
<img width="1278" alt="Screen Shot 2022-07-13 at 10 44 32 PM" src="https://user-images.githubusercontent.com/9786713/178893549-c2d54857-603c-46f9-8ea5-555621c5cbad.png">
