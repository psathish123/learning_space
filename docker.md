> **Reference site: https://www.tutorialspoint.com/docker/**
 

1. **Docker install**

        * install via rpm / debian package

2. **Docker image download**
        
        * docker images
        * docker pull <image_name>

                * docker pull jenkins/jenkins:lts
        
        * docker images


3. **Docker image run**

        * docker ps 
        * docker run -p localport:containerport <image_name>
                        
                * docker run -p 8080:8080 -p 50000:50000 docker.io/jenkins/jenkins:lts

        * docker ps 
     
     View all containers 
     
        * docker ps -a

4. **Access the container application from base machine**

        * Access your application from your web browser chrome

        * http://127.0.0.1:8080

5. **Inspect docker image or container**

       inspect a image
       
              * docker inspect <image_id>
              
       inspect a container
       
              * docker inspect <container_id>

6. **Create own Docker image**
```

# mkdir myapp
# cd myapp/

# echo "echo Myscript" >> ./myscript.sh 

# cat Dockerfile
FROM centos
MAINTAINER Sathishkumar
LABEL version="1.0"
LABEL description="First image with Dockerfile."
COPY ./myscript.sh /root

# docker build -t mydockerimage:v1.0 .


View your image

# docker images

```

**Docker Build sample output**
```

# docker build -t sathishimg:v3.0 .
Sending build context to Docker daemon  4.096kB
Step 1/6 : FROM centos
 ---> 5d0da3dc9764
Step 2/6 : MAINTAINER Sathishkumar
 ---> Using cache
 ---> 2fb45f40d944
Step 3/6 : LABEL version="1.0"
 ---> Using cache
 ---> 076bfdbb8f61
Step 4/6 : LABEL description="First image with Dockerfile."
 ---> Using cache
 ---> 4594487a28d3
Step 5/6 : COPY ./myscript.sh /root
 ---> Using cache
 ---> 8e993c231414
Step 6/6 : ADD bashrc /root/.bashrc
 ---> Using cache
 ---> b9e660b8349c
Successfully built b9e660b8349c
Successfully tagged sathishimg:v3.0


# docker images
REPOSITORY        TAG       IMAGE ID       CREATED              SIZE
sathishimg        v3.0      b9e660b8349c   About a minute ago   231MB

```
