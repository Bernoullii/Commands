# Jenkins Installation

#### source: https://jenkins.io/doc/book/installing/

#### Create a bridge network in Docker
`docker network create jenkins`

#### Create the following volumes to share the Docker client TLS certificates needed to connect to the Docker daemon and persist the Jenkins data
`docker volume create jenkins-docker-certs`

`docker volume create jenkins-data`

#### Pull dind image
`docker image pull docker:dind`


#### Pull jenkins image
`docker image pull jenkinsci/blueocean`

#### Run dind
`docker container run --name jenkins-docker --rm --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 docker:dind`

#### Run jenkinsci/blueocean
`docker container run --name jenkins-blueocean --rm --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --publish 8080:8080 --publish 50000:50000 jenkinsci/blueocean`


#### When you first access a new Jenkins instance, you are asked to unlock it using an automatically-generated password.
`docker logs jenkins-blueocean`


#### installation bash script:

`#!/bin/bash`

#### source : https://wiki.jenkins.io/display/JENKINS/Installing+Jenkins+with+Docker#space-menu-link-content

#### Pull latest container
`docker pull jenkins`

#### Setup local configuration folder
#### Should already be in a jenkins folder when running this script.
`export CONFIG_FOLDER=$PWD/config
mkdir $CONFIG_FOLDER
chown 1000 $CONFIG_FOLDER`

#### Start container
`docker run --restart=always -d -p 49001:8080 \
-v $CONFIG_FOLDER:/var/jenkins_home:z \
--name jenkins -t jenkins`

`docker logs --follow jenkins`