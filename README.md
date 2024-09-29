# ASSIGNMENT 2: Automating CI/CD with Dockerized Jenkins: Secure Pipeline for Node.js Web Application Deployment

Docker Compose Setup for Jenkins and Docker-in-Docker (DinD)
This project utilizes Docker Compose to set up a Jenkins CI/CD server alongside a Docker-in-Docker (DinD) service. This configuration allows for efficient continuous integration and deployment workflows, leveraging Jenkins for automation and Docker for containerized environments.

# Table of Contents
Prerequisites
Installation
Configuration
Usage
Services
Networking
Volumes
License

# Prerequisites
Docker and Docker Compose installed on your machine.
Basic knowledge of Docker and Jenkins.
Access to a terminal or command line interface.

# Installation
1. Clone the repository or create a directory for the Docker Compose setup:
git clone <repository-url>
cd <directory>

2. Create the necessary directories for Jenkins data persistence:
mkdir jenkins
3. Place your Docker Compose file (docker-compose.yml) in the directory.

# Configuration
The docker-compose.yml file is configured as follows:
Jenkins runs on the official Jenkins image with the JDK 11 variant.
Docker-in-Docker (DinD) allows Jenkins to build Docker images and run containers.

# Usage
To start the services, run the following command in the directory containing the docker-compose.yml file:
docker compose up

You can access Jenkins at http://localhost:8080. To get the initial admin password, run:
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
or see in docker logs jenkins / console output 

# Services
## Jenkins
- Image: jenkins/jenkins:lts-jdk11
- Ports:
  - 8080: Jenkins web interface
  - 50000: Jenkins agent communication
- Volumes:
  - ./jenkins:/var/jenkins_home: Persists Jenkins data.
  - /usr/bin/docker:/usr/bin/docker: Provides access to the Docker binary.
  - /var/run/docker.sock:/var/run/docker.sock: Allows Jenkins to communicate with the Docker daemon.
  - $HOME:/home: Mounts the home directory.
## Docker-in-Docker (DinD)
- Image: docker:dind
- Ports:
  - 2376: Exposes Docker daemon for TLS communication.
- Environment:
  - DOCKER_TLS_CERTDIR=/certs: Configures the directory for TLS certificates.
- Volumes:
  - jenkins-data:/var/jenkins_home: Shares data between Jenkins and DinD.
  - jenkins-docker-certs:/certs/client: Stores Docker TLS certificates.
- Networking
Both Jenkins and DinD services are connected to a user-defined Docker network called docker. This network enables communication between the services.

## Volumes
- Two named volumes are created for persistent data storage:
  - jenkins-data: Stores Jenkins home directory data.
  - jenkins-docker-certs: Stores TLS certificates for Docker-in-Docker.

# License
This project is licensed under the MIT License. See the LICENSE file for details.
