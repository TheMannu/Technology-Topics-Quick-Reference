Scripting Approach to download and install-docker

  - sudo -i
  - curl -fsSL https://get.docker.com -o install-docker.sh
  - ls
  - sudo sh install-docker.sh

Debian or ubuntu Approach to download and install-docker

  - sudo —i
  - apt-get update
  - apt install docker. io
  - docker —v

- docker pull hello-world 
- docker images
- docker run hello-world
- docker ps  => running processes 
- docker ps -a => -a is all or archives
---
### Installing Docker compose for container 

  - [Docker & Docker Compose](https://docs.google.com/document/d/1hjn_wEQLecAplZNsgGDHv5yBoWa3dofU_kVT7qJrK7U/edit?tab=t.0)
---
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.




Docker pull  Vs Docker run 

pull - download the image to our system
run - download the image + container will be created 


- docker run -it ubuntu => -it is interactive mode and moves inside the container at the begning
- exit 

- docker ps -a 
- docker start CONTAINER ID
- docker logs CONTAINER ID
- docker inspect CONTAINER ID
- docker rm CONTAINER ID
- docker exec -it CONTAINER ID bin/bash => to move inside the container at any time after starting
- docker stop CONTAINER ID


ubuntu image = 78 mb             
ubuntu operatig system = 2 GB extcutale file 6gb run

Smallest size of the Operating System Image  =>  Alpine 

Container life cycle and there commands

  • docker pause <container ID>
	docker pause command suspends all processes in the specified container.

	• docker stop <container ID>
	docker stop command will kill main process inside the container.

	• docke kill <container ID>
	docker kill command will kill the container.

	• docker rm -f <container ID>
	docker rm command will remove the container.

	• docker rmi -f <image name>
	docker rmi command will remove the image itself.

	• docker start <container ID>

		
- docker run nginx => the nginx is working fine but not reachable from browser.

- docker run -p 80:80 nginx  => to configure port 80 of nginx to port 80 of machine.

- docker run --name docker-nginx -p 80:80 nginx => to assign a name to the container.

 ---------------------------
mkdir -p ~/docker-nginx/html
cd ~/docker-nginx/html
vi index.html     
----------------------------

<html>
  <head>
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" rel="stylesheet" integrity="sha256-MfvZlkHCEqatNoGiOXveE8FIwMzZg4W85qfrfIFBfYc= sha512-dTfge/zgoMYpP7QbHy4gWMEGsbsdZeCXz7irItjcC3sPUFtf0kuFbDz/ixG7ArTxmDjLXDmezHubeNikyKGVyQ==" crossorigin="anonymous">
    <title>Docker nginx Tutorial</title>
  </head>
  <body>
    <div class="container">
      <h1>Hello Learning team  with Devanshau , Debyan , sabir , Nisha</h1>
      <p>This nginx page is brought to you by Docker in front of sri ram arun ravi naveen sabita </p>
    </div>
  </body>
</html>

-------------
- docker run --name docker-nginx -p 80:80 -d -v ~/docker-nginx/html:/usr/share/nginx/html nginx
----------------------------

Jenkins Installation

- docker run —it —d —p 8080: 8080 jenkins/jenkins:latest
-------------
- sudo cat /var/jenkins_home/secrets/initialAdminPassword
----------------------------

vi Dockerfile
  FROM ubuntu
  MAINTAINER clouddevopshub@gmail . com
  RUN apt—get update                         => OS level command
  RUN apt—get install nginx —y               => OS level command
  CMD ["echo","Image created"]               => Application level command

Buildig Image

  - docker build —t mynginxbatch35 .           => will create image in current directery with tagname(-t) mynginxbatch35

Push to docker Hub
  - docker login 
  - docker push themannu/mynginxbatch35
  - docker pull themannu/mynginxbatch35       => to varify the images is on docker hub

Docker file Blog
- https://www.clouddevopshub.com/blog/dockerfile

- https://github.com/komljen/dockerfile-examples

Making Image of Runnig Container
  First run the Container 
  - docker run -d —p 80:80 nginx
- docker commit Container-ID <new image name>

Taking backup of image
- docker save Container-name > mybackup.tar
- ls 
- aws s3 cp   => to move the backup file to s3 storage

Docker troubleshooting / monitoring commands
- docker inspect <container name>
- docker inspect <container id>
- docker logs <container name>
- docker logs <container id>
- docker logs -f <container name>  => real time log
- docker stats <container ID>
- docker top <container name>
- docker top <container id>

Cleanup or prune unused (dangling) images
- docker system df
- docker image prune -a