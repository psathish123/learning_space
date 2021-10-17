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
