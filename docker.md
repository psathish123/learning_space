## Docker Learning

> **Reference site: https://www.tutorialspoint.com/docker/**
 
## **Docker install**

    install via rpm / debian package

## **Docker image download**

```sh
        docker images
        docker pull <image_name>
```
Example
```sh
        docker pull jenkins/jenkins:lts
        docker images
```
## **Docker image run**

```sh
 docker ps 
 docker run -p localport:containerport <image_name>
```  
Example
```
 docker run -p 8080:8080 -p 50000:50000 docker.io/jenkins/jenkins:lts
```
View docker containers
```
docker ps      
docker ps -a
```

## **Access the container application from base machine**

     Access your application from your web browser chrome
     http://127.0.0.1:8080

## **Inspect docker image or container**
Inspect docker image

```       
docker inspect <image_id>
```              
Inspect a container

```
 docker inspect <container_id>
```

## **Create own Docker image**
```sh
# mkdir myapp
# cd myapp/
# echo "echo Myscript" >> ./myscript.sh 

# cat Dockerfile
FROM centos
MAINTAINER Sathishkumar
LABEL version="1.0"
LABEL description="First image with Dockerfile."
COPY ./myscript.sh /root


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

## **Docker - Working with Containers**

**docker top**

With this command, you can see the top processes within a container.

#### Syntax

    docker top ContainerID 
   
```sh
[root@ip-172-31-87-150 ~]# docker run -it centos:latest /bin/bash
[root@da81b50a1241 ~]# sleep 60
```

Check in other putty session
```sh
[root@ip-172-31-87-150 ~]# docker ps
CONTAINER ID   IMAGE           COMMAND       CREATED          STATUS          PORTS     NAMES
da81b50a1241   centos:latest   "/bin/bash"   55 seconds ago   Up 53 seconds             naughty_ptolemy
```

To see all process in container
```sh
[root@ip-172-31-87-150 ~]# docker top da81b50a1241
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                30067               30025               0                   05:55               pts/0               00:00:00            /bin/bash
root                30255               30067               0                   05:59               pts/0               00:00:00            /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 60
```

## **docker stats**

This command is used to provide the statistics of a running container.

Syntax
    
    docker stats ContainerID 

example:
```sh
[root@ip-172-31-87-150 ~]# docker stats b87d1b958380
CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O     BLOCK I/O     PIDS
b87d1b958380   nice_poincare   0.00%     2.836MiB / 983.3MiB   0.29%     960B / 0B   3.06MB / 0B   1
```

## **docker attach**

This command is used to attach to a running container.

Syntax

	docker attach ContainerID 

example:
```sh
[root@ip-172-31-87-150 ~]# docker attach b87d1b958
[root@b87d1b958380 /]#
```

## **docker pause**

This command is used to pause the processes in a running container.

Syntax

	docker pause ContainerID 

example:

```
[root@ip-172-31-87-150 ~]# docker ps
CONTAINER ID   IMAGE           COMMAND       CREATED          STATUS         PORTS     NAMES
b87d1b958380   centos:latest   "/bin/bash"   12 minutes ago   Up 3 minutes             nice_poincare

[root@ip-172-31-87-150 ~]# docker pause nice_poincare
nice_poincare

[root@ip-172-31-87-150 ~]# docker ps
CONTAINER ID   IMAGE           COMMAND       CREATED          STATUS                  PORTS     NAMES
b87d1b958380   centos:latest   "/bin/bash"   15 minutes ago   Up 6 minutes (Paused)             nice_poincare
```
## **docker unpause**

This command is used to unpause the processes in a running container.

Syntax
    
    docker unpause ContainerID

Example
```sh
[root@ip-172-31-87-150 ~]# docker unpause nice_poincare
nice_poincare

[root@ip-172-31-87-150 ~]# docker ps
CONTAINER ID   IMAGE           COMMAND       CREATED          STATUS          PORTS     NAMES
b87d1b958380   centos:latest   "/bin/bash"   30 minutes ago   Up 21 minutes             nice_poincare
```

## **docker kill**

This command is used to kill the processes in a running container. We can run the command when "docker stop" not work

Syntax

	docker kill ContainerID	

Example
```sh
[root@ip-172-31-87-150 ~]# docker kill nice_poincare
nice_poincare

[root@ip-172-31-87-150 ~]# docker ps -a
CONTAINER ID   IMAGE           COMMAND       CREATED          STATUS                       PORTS     NAMES
b87d1b958380   centos:latest   "/bin/bash"   31 minutes ago   Exited (137) 9 seconds ago             nice_poincare
```



