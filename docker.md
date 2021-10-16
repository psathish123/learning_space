```
1. Docker install

        * install via rpm / debian package

2. Docker image download
        
        * docker images
        * docker pull <image_name>

                * docker pull jenkins/jenkins:lts
        
        * docker images


3. Docker image run

        * docker ps 
        * docker run -p localport:containerport <image_name>
                        
                * docker run -p 8080:8080 -p 50000:50000 docker.io/jenkins/jenkins:lts

        * docker ps 

4. Access the container application from base machine

        * Access your application from your web browser chrome

        * http://127.0.0.1:8080
