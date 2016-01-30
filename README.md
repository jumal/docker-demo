# Docker Demo [![Build Status](https://travis-ci.org/jumal/docker-demo.svg?branch=master)](https://travis-ci.org/jumal/docker-demo)

Docker demo using 3 containers types:
 - **web** running a REST API
 - **mongodb** running a MongoDB server with data persistence
 - **ha_web** running a load balancer for the web containers

## Installing Docker
Follow the instructions for your OS on [this page](https://docs.docker.com/engine/installation).

## Building the REST API
`mvn clean package`

## Starting the Containers
`docker-compose up -d`

List the running containers

`docker-compose ps`

## Scaling
Create 3 instances of the web container

`docker-compose scale web=3`

List the running containers

`docker-compose ps`

## Accessing the REST API
Point your broweser to http://127.0.0.1

*Note:* if using boot2docker, use the IP contained in the DOCKER_HOST environment variable. 

## Miscellaneous

#### Connecting to a Container
`docker exec -it dockerdemo_web_1 bash`

#### Viewing a Container's logs
`docker logs dockerdemo_web_1`

#### Stopping and Removing the Containers
`docker-compose stop && docker-compose rm -f`

#### Updating a Container Image
`docker-compose build`
