# Docker Demo

Docker demo using 3 containers
 - *web* hosting a REST API
 - *mongodb* hosting a MongoDB server with data persistence
 - *ha_web* hosting a load balancer for the web containers

## Installing Docker

First, you need to [install Docker](https://docs.docker.com/engine/installation).

## Building the REST API
`mvn clean package`

## Starting the containers
`docker-compose up -d`

List the running containers by typing
`docker-compose ps`

## Scaling
Create 3 instances of the web container by typing
`docker-compose scale web=3`

List the running containers by typing
`docker-compose ps`

## Miscellaneous

#### Connecting to a container
`docker exec -it dockerdemo_web_1 bash`

#### Viewing a container logs
`docker logs dockerdemo_web_1`

#### Stopping and removing the containers
`docker-compose stop && docker-compose rm -f`

#### Rebuilding the web image
`docker-compose build`
