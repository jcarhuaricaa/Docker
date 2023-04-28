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

![image](https://user-images.githubusercontent.com/9786713/179102525-87fba4c2-85a6-4a61-b851-5a82e04b10d5.png)

![image](https://user-images.githubusercontent.com/9786713/179101266-334c7ba2-8da1-42a6-8b0f-01390c8489d2.png)

![image](https://user-images.githubusercontent.com/9786713/179101325-aee56b4f-a089-4864-8cb2-47ebebae92f9.png)

### 7. Commands to create image for Dockerfile4.
Try to have less layers as much as possible
```bash
docker build -t test:v2 -f Dockerfile4 .
docker run -d -p 9090:80 test:v2
```

<img width="637" alt="Screen Shot 2022-07-18 at 10 03 50 PM" src="https://user-images.githubusercontent.com/9786713/179655463-ad98832c-c6b6-4d9a-b494-440e57f20423.png">

### 8. Going to ssl folder and create the image for Dockerfile.

First, the ssl certificate need to be created with the following command:
resource (https://major.io/2007/08/02/generate-self-signed-certificate-and-key-in-one-line/)

```bash
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout docker.key -out docker.crt
```

Now we need to tell apache use the certificates created before.
resource (https://www.techrepublic.com/article/how-to-enable-https-on-apache-centos/
https://cwiki.apache.org/confluence/display/HTTPD/NameBasedSSLVHosts)

according to the resource we need to install openssl and add the some lines in the ssl.conf file.

```bash
docker build -t apache_ssl:v3 .
docker run -d -p 443:443 --name apache_ssl apache_ssl:v3
```

### 9. Dangling image.
In order to avoid dangling image being created it's importan to use tags with the name of our images when we created a new version.

<img width="893" alt="image" src="https://user-images.githubusercontent.com/9786713/180916789-181497c0-e0c1-40a3-baa0-390caf36f760.png">

Now to delete all dangling images in one command execute the following command:

```bash
docker rmi $(docker images -f dangling=true -q)
```

<img width="893" alt="Screen Shot 2022-07-25 at 10 36 01 PM" src="https://user-images.githubusercontent.com/9786713/180917611-60b01319-7ab8-4065-99d5-dce016eeb534.png">

### 10. Installing Nginx and PHP 7 on centos 7

resource (https://www.cyberciti.biz/faq/how-to-install-and-use-nginx-on-centos-7-rhel-7/, https://ius.io/setup)

We are going to create the nginx.repo file with the content on the link before. Also we are going to install the ius repository in order to install the php7 package.

Now we need to setup the nginx for php to work. So, we are going to create the nginx.conf using the following template:
resource (https://gist.github.com/lukearmstrong/7155390)

Now we create start.sh file just to start the php process on background and the nginx on foreground. Then we want to create a simple test.php file just to validate php works. If we create and image and container, It will fail because of the line 17 on the  nginx.conf file. For that we will fix that by changing that line according the following resource.

https://serversforhackers.com/c/php-fpm-configuration-the-listen-directive

Finally we are going to rebuild the image and create the new container and it works as expected.

### 11. Going to multi-stage folder and create the image 

For this example the last FROM is which persist on the size of the image.

<img width="893" alt="image" src="https://user-images.githubusercontent.com/9786713/182756808-d619995c-7e6b-4423-a334-99b0106d9a57.png">

creating the image for Dockerfile-multistage, this is a similar example but with a real use case:
resource: (https://github.com/jenkins-docs/simple-java-maven-app)

<img width="893" alt="image" src="https://user-images.githubusercontent.com/9786713/182763658-b840c58f-2495-4ed2-a5dc-14529812424a.png">

<img width="893" alt="Screen Shot 2022-08-03 at 11 38 43 PM" src="https://user-images.githubusercontent.com/9786713/182764398-bb1d6035-c893-428f-ad53-1db26c6252b6.png">

### 12. Commands for containers (rename,start,stop)

<img width="893" alt="Screen Shot 2022-08-15 at 11 02 56 PM" src="https://user-images.githubusercontent.com/9786713/184795384-94c9a420-b0c1-49fb-82ac-f582ed65d4f9.png">

<img width="893" alt="image" src="https://user-images.githubusercontent.com/9786713/184795731-3109529b-c167-4321-a577-07f9e3d717cc.png">

<img width="893" alt="Screen Shot 2022-08-15 at 11 48 32 PM" src="https://user-images.githubusercontent.com/9786713/184800014-5e711888-6387-4c37-a36d-5e0159ff9587.png">

<img width="1454" alt="Screen Shot 2022-08-15 at 11 51 01 PM" src="https://user-images.githubusercontent.com/9786713/184800287-e65f1651-81e4-4dee-b032-58ed3f9bfdc1.png">

<img width="893" alt="Screen Shot 2022-08-15 at 11 55 50 PM" src="https://user-images.githubusercontent.com/9786713/184800854-d2894104-0d29-433b-8c57-313399e07e66.png">

<img width="893" alt="Screen Shot 2022-08-15 at 11 58 15 PM" src="https://user-images.githubusercontent.com/9786713/184801116-064cb965-6ffa-4174-b0e9-79cbf6e54911.png">

<img width="893" alt="Screen Shot 2022-08-16 at 12 15 20 AM" src="https://user-images.githubusercontent.com/9786713/184802974-e6409ff8-f69f-4a8f-b9d5-cfbfa0836218.png">

