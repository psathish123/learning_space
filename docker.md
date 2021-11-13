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

###### 13/November/2021


## Manage docker service

```sh
# systemctl start docker
# systemctl status docker
# systemctl stop docker
```


## nsenter command to run a commands in docker container

```sh
[root@gateway ~]# docker run -it centos /bin/bash

[root@2b2719609286 /]# 
[root@gateway ~]# 

[root@gateway ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
2b2719609286   centos    "/bin/bash"   12 seconds ago   Up 10 seconds             nervous_pasteur
[root@gateway ~]# 



[root@gateway ~]# docker inspect quirky_black | grep Pid
            "Pid": 20892,
            
[root@gateway ~]# /usr/bin/nsenter -m -u -n -p -i -t 20892 /bin/bash
```


## Create own docker image with web application

```sh
[root@gateway ~ ]# cd mywebapp1/

[root@gateway mywebapp1]# cat Dockerfile 
FROM centos
MAINTAINER sathish@example.com
RUN yum install httpd -y
CMD ["echo","Image Created"]
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
EXPOSE 80
[root@gateway mywebapp1]# 

```

## Perform your docker build


```sh
[root@gateway mywebapp1]# docker build -t mywebapp:0.1 . 
Sending build context to Docker daemon  2.048kB
Step 1/6 : FROM centos
 ---> 5d0da3dc9764
Step 2/6 : MAINTAINER sathish@example.com
 ---> Running in 8dd1582bbf98
Removing intermediate container 8dd1582bbf98
 ---> 4f40ab65a6dd
Step 3/6 : RUN yum install httpd -y
 ---> Running in fdf43d9494a8
CentOS Linux 8 - AppStream                      1.6 MB/s | 9.6 MB     00:05    
CentOS Linux 8 - BaseOS                         1.7 MB/s | 8.5 MB     00:04    
CentOS Linux 8 - Extras                          15 kB/s |  10 kB     00:00    
Last metadata expiration check: 0:00:01 ago on Sat Nov 13 06:33:13 2021.
Dependencies resolved.
======================================================================================
 Package              Arch    Version                                 Repo        Size
======================================================================================
Installing:
 httpd                x86_64  2.4.37-39.module_el8.4.0+950+0577e6ac.1 appstream  1.4 M
Installing dependencies:
 apr                  x86_64  1.6.3-11.el8                            appstream  125 k
 apr-util             x86_64  1.6.1-6.el8                             appstream  105 k
 brotli               x86_64  1.0.6-3.el8                             baseos     323 k
 centos-logos-httpd   noarch  85.8-1.el8                              baseos      75 k
 httpd-filesystem     noarch  2.4.37-39.module_el8.4.0+950+0577e6ac.1 appstream   39 k
 httpd-tools          x86_64  2.4.37-39.module_el8.4.0+950+0577e6ac.1 appstream  106 k
 mailcap              noarch  2.1.48-3.el8                            baseos      39 k
 mod_http2            x86_64  1.15.7-3.module_el8.4.0+778+c970deab    appstream  154 k
Installing weak dependencies:
 apr-util-bdb         x86_64  1.6.1-6.el8                             appstream   25 k
 apr-util-openssl     x86_64  1.6.1-6.el8                             appstream   27 k
Enabling module streams:
 httpd                        2.4                                                     

Transaction Summary
======================================================================================
Install  11 Packages

Total download size: 2.4 M
Installed size: 7.1 M
Downloading Packages:
(1/11): apr-util-bdb-1.6.1-6.el8.x86_64.rpm      85 kB/s |  25 kB     00:00    
(2/11): apr-util-openssl-1.6.1-6.el8.x86_64.rpm 190 kB/s |  27 kB     00:00    
(3/11): apr-1.6.3-11.el8.x86_64.rpm             251 kB/s | 125 kB     00:00    
(4/11): apr-util-1.6.1-6.el8.x86_64.rpm         207 kB/s | 105 kB     00:00    
(5/11): httpd-filesystem-2.4.37-39.module_el8.4 373 kB/s |  39 kB     00:00    
(6/11): httpd-tools-2.4.37-39.module_el8.4.0+95 739 kB/s | 106 kB     00:00    
(7/11): brotli-1.0.6-3.el8.x86_64.rpm           1.0 MB/s | 323 kB     00:00    
(8/11): mod_http2-1.15.7-3.module_el8.4.0+778+c 419 kB/s | 154 kB     00:00    
(9/11): mailcap-2.1.48-3.el8.noarch.rpm         229 kB/s |  39 kB     00:00    
(10/11): centos-logos-httpd-85.8-1.el8.noarch.r 394 kB/s |  75 kB     00:00    
(11/11): httpd-2.4.37-39.module_el8.4.0+950+057 1.6 MB/s | 1.4 MB     00:00    
--------------------------------------------------------------------------------
Total                                           1.0 MB/s | 2.4 MB     00:02     
warning: /var/cache/dnf/appstream-02e86d1c976ab532/packages/apr-1.6.3-11.el8.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID 8483c65d: NOKEY
CentOS Linux 8 - AppStream                      1.6 MB/s | 1.6 kB     00:00    
Importing GPG key 0x8483C65D:
 Userid     : "CentOS (CentOS Official Signing Key) <security@centos.org>"
 Fingerprint: 99DB 70FA E1D7 CE22 7FB6 4882 05B5 55B3 8483 C65D
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1 
  Installing       : apr-1.6.3-11.el8.x86_64                               1/11 
  Running scriptlet: apr-1.6.3-11.el8.x86_64                               1/11 
  Installing       : apr-util-bdb-1.6.1-6.el8.x86_64                       2/11 
  Installing       : apr-util-openssl-1.6.1-6.el8.x86_64                   3/11 
  Installing       : apr-util-1.6.1-6.el8.x86_64                           4/11 
  Running scriptlet: apr-util-1.6.1-6.el8.x86_64                           4/11 
  Installing       : httpd-tools-2.4.37-39.module_el8.4.0+950+0577e6ac.    5/11 
  Installing       : mailcap-2.1.48-3.el8.noarch                           6/11 
  Installing       : centos-logos-httpd-85.8-1.el8.noarch                  7/11 
  Installing       : brotli-1.0.6-3.el8.x86_64                             8/11 
  Running scriptlet: httpd-filesystem-2.4.37-39.module_el8.4.0+950+0577    9/11 
  Installing       : httpd-filesystem-2.4.37-39.module_el8.4.0+950+0577    9/11 
  Installing       : mod_http2-1.15.7-3.module_el8.4.0+778+c970deab.x86   10/11 
  Installing       : httpd-2.4.37-39.module_el8.4.0+950+0577e6ac.1.x86_   11/11 
  Running scriptlet: httpd-2.4.37-39.module_el8.4.0+950+0577e6ac.1.x86_   11/11 
  Verifying        : apr-1.6.3-11.el8.x86_64                               1/11 
  Verifying        : apr-util-1.6.1-6.el8.x86_64                           2/11 
  Verifying        : apr-util-bdb-1.6.1-6.el8.x86_64                       3/11 
  Verifying        : apr-util-openssl-1.6.1-6.el8.x86_64                   4/11 
  Verifying        : httpd-2.4.37-39.module_el8.4.0+950+0577e6ac.1.x86_    5/11 
  Verifying        : httpd-filesystem-2.4.37-39.module_el8.4.0+950+0577    6/11 
  Verifying        : httpd-tools-2.4.37-39.module_el8.4.0+950+0577e6ac.    7/11 
  Verifying        : mod_http2-1.15.7-3.module_el8.4.0+778+c970deab.x86    8/11 
  Verifying        : brotli-1.0.6-3.el8.x86_64                             9/11 
  Verifying        : centos-logos-httpd-85.8-1.el8.noarch                 10/11 
  Verifying        : mailcap-2.1.48-3.el8.noarch                          11/11 

Installed:
  apr-1.6.3-11.el8.x86_64                                                       
  apr-util-1.6.1-6.el8.x86_64                                                   
  apr-util-bdb-1.6.1-6.el8.x86_64                                               
  apr-util-openssl-1.6.1-6.el8.x86_64                                           
  brotli-1.0.6-3.el8.x86_64                                                     
  centos-logos-httpd-85.8-1.el8.noarch                                          
  httpd-2.4.37-39.module_el8.4.0+950+0577e6ac.1.x86_64                          
  httpd-filesystem-2.4.37-39.module_el8.4.0+950+0577e6ac.1.noarch               
  httpd-tools-2.4.37-39.module_el8.4.0+950+0577e6ac.1.x86_64                    
  mailcap-2.1.48-3.el8.noarch                                                   
  mod_http2-1.15.7-3.module_el8.4.0+778+c970deab.x86_64                         

Complete!
Removing intermediate container fdf43d9494a8
 ---> aeba27204b4e
 
 
[root@gateway mywebapp1]# docker history mywebapp:0.1 
IMAGE          CREATED              CREATED BY                                      SIZE      COMMENT
b909aea77df8   About a minute ago   /bin/sh -c #(nop)  EXPOSE 80                    0B        
0c87646685f9   About a minute ago   /bin/sh -c #(nop)  CMD ["/usr/sbin/httpd" "-…   0B        
8d0fe1941021   About a minute ago   /bin/sh -c #(nop)  CMD ["echo" "Image Create…   0B        
aeba27204b4e   2 minutes ago        /bin/sh -c yum install httpd -y                 57MB      
4f40ab65a6dd   2 minutes ago        /bin/sh -c #(nop)  MAINTAINER sathish@exampl…   0B        
5d0da3dc9764   8 weeks ago          /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      8 weeks ago          /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B        
<missing>      8 weeks ago          /bin/sh -c #(nop) ADD file:805cb5e15fb6e0bb0…   231MB     
[root@gateway mywebapp1]# 

```

# Usecase 1 : Spin up your 1st container application

```sh

[root@gateway ~]# docker images
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
mywebapp                0.1       b909aea77df8   55 minutes ago   288MB


[root@gateway mywebapp1]# docker run -i -t -d mywebapp:0.1 
d33a204287e4d955f6fc51368123158f10eb420b8a40fb160228ee40f84abaca
[root@gateway mywebapp1]# 


[root@gateway mywebapp1]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS          PORTS     NAMES
d33a204287e4   mywebapp:0.1   "/usr/sbin/httpd -D …"   18 seconds ago      Up 16 seconds   80/tcp    gracious_bhaskara


[root@gateway mywebapp1]# docker inspect gracious_bhaskara | grep Pid
            "Pid": 25918,


root@gateway mywebapp1]# /usr/bin/nsenter -m -u -n -p -i -t 25918 /bin/sh

sh-4.4# ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 06:38 pts/0    00:00:00 /usr/sbin/httpd -D FOREGROUND
apache         7       1  0 06:38 pts/0    00:00:00 /usr/sbin/httpd -D FOREGROUND
apache         8       1  0 06:38 pts/0    00:00:00 /usr/sbin/httpd -D FOREGROUND
apache         9       1  0 06:38 pts/0    00:00:00 /usr/sbin/httpd -D FOREGROUND
apache        10       1  0 06:38 pts/0    00:00:00 /usr/sbin/httpd -D FOREGROUND
root         222       0  0 06:39 ?        00:00:00 /bin/sh
root         225     222  0 06:40 ?        00:00:00 ps -ef
sh-4.4#

```

# Usecase 2 : Run your container with external port 

```sh
[root@gateway mywebapp1]# docker run -i -t -d -p 8585:80 mywebapp:0.1 
6aa048c1d48e7ff0a03315d3a78b525f23cc88107b012e28785b428ef2922618

[root@gateway mywebapp1]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS          PORTS                                   NAMES
6aa048c1d48e   mywebapp:0.1   "/usr/sbin/httpd -D …"   4 seconds ago       Up 2 seconds    0.0.0.0:8585->80/tcp, :::8585->80/tcp   jovial_edison


[root@gateway mywebapp1]# netstat -tunlp | grep 8585
tcp        0      0 0.0.0.0:8585            0.0.0.0:*               LISTEN      26885/docker-proxy  


access web app from your web browser 
http://192.168.1.104:8585/

```

# Usecase 3 : Run your container with external port with attached local volume in "Document Root" path

```sh
[root@gateway mywebapp1]# docker run -i -t -d -p 9090:80 -v /home/sathish/webapps:/var/www/html mywebapp:0.1 
05006a87d593a1c0d33407848919952b5ae7af3bedfaab5c44a7080a867b087e


[root@gateway mywebapp1]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS          PORTS                                   NAMES
05006a87d593   mywebapp:0.1   "/usr/sbin/httpd -D …"   12 seconds ago      Up 10 seconds   0.0.0.0:9090->80/tcp, :::9090->80/tcp   elegant_easley


access web app from your web browser 
http://192.168.1.104:9090/

```

# Update new website in your web application

```sh

[root@gateway webapps]# cd /home/sathish/webapps/

[root@gateway webapps]# cat index.html 

<h1> My first web application <h1>

<h2> I am excited on this <h2>

[root@gateway webapps]# 


[root@gateway ~]# docker inspect elegant_easley | grep Pid
            "Pid": 27919,
            

[root@gateway ~]# /usr/bin/nsenter -m -u -n -p -i -t 27919 /bin/bash
[root@05006a87d593 /]# 


[root@05006a87d593 /]# df  
Filesystem     1K-blocks     Used Available Use% Mounted on
overlay         82438552 30007804  48220060  39% /
tmpfs              65536        0     65536   0% /dev
shm                65536        0     65536   0% /dev/shm
/dev/sda5       82438552 30007804  48220060  39% /etc/hosts
/dev/sda6       82438552 74295636   3932228  95% /var/www/html
tmpfs            3935088        0   3935088   0% /proc/asound
tmpfs            3935088        0   3935088   0% /proc/acpi
tmpfs            3935088        0   3935088   0% /proc/scsi
tmpfs            3935088        0   3935088   0% /sys/firmware
[root@05006a87d593 /]# cd /var/www/html/
[root@05006a87d593 html]# ls
index.html

[root@05006a87d593 html]# ls -l
total 4
-rw-r--r-- 1 root root 309 Nov 13 06:57 index.html

```


