# Docker Demo [![Build Status](https://travis-ci.org/jumal/docker-demo.svg?branch=master)](https://travis-ci.org/jumal/docker-demo)

Docker demo using 5 containers:
 - 3 **web** containers running a REST API
 - 1 **mongodb** server with data persistence
 - 1 **load_balancer** for the web containers

## Instructions

### Install Docker
Follow the instructions for your OS on [this page](https://docs.docker.com/engine/installation).

### Build the REST API
```
mvn clean package
```
### Set the Number of Containers
```
docker-compose scale web=3
```
### Start the Containers
```
docker-compose up -d
```
List the running containers
```
docker-compose ps
```
### Access the REST API
Point your browser to http://127.0.0.1.

*Note:* if using boot2docker, use the IP contained in the DOCKER_HOST environment variable. 
## Configuration
### Docker
File: *Dockerfile*
```
FROM debian

RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee /etc/apt/sources.list.d/webupd8team-java.list && \
    echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886 && \
    apt-get update && \
    echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
    apt-get install -y oracle-java8-installer oracle-java8-set-default && \
    apt-get clean && \
    rm -rf /var/cache/oracle-jdk8-installer && \
    rm -rf /var/lib/apt/lists/*
    
RUN groupadd -r docker && useradd -rmg docker docker
VOLUME /tmp
WORKDIR /home/docker
ADD target/*.jar app.jar
RUN touch app.jar
USER docker
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","app.jar"]
```
### Docker Compose
File: *docker-compose.yml*
```
load_balancer:
 image: tutum/haproxy
 links:
   - web
 ports:
   - "80:80"
web:
  build: .
  ports:
   - "8080"
  links:
   - mongodb
mongodb:
  image: mongo
```
## HOWTOs
### Connect to a Container
```
docker exec -it dockerdemo_web_1 bash
```
### View a Container's logs
```
docker logs dockerdemo_web_1
```
### Add more Containers
Create 2 more instances of the web container
```
docker-compose scale web=5 && docker-compose up --force-recreate -d
```
### Stop the Containers
```
docker-compose stop
```
### Update a Container Image
```
docker-compose build
```
### Remove the Containers
```
docker-compose rm -f
```
