# Continuous Delivery

The aim of this project is to deliver a live demo environment for showing working examples of Continuous Delivery.
It consist of an ecosystem of a CoreOS cluster and a set of Docker containers that together provides the following servers:

- Jenkins
- Nexus
- Sonar
- Protractor + Selenium
- Tomcat (Test and Prod)


### Overview of the CD ecosystem
<img src="images/cd-ecosystem-overview.png" alt="cd-ecosystem-overview" style="max-width:50%"/> 

## Getting Started

### Prerequisites
1. Virtualbox installed, version >= 4.3.12 < 5.0
1. Vagrant installed, version == 1.7.2
1. (Windows Only) Cygwin with rsync binary (selectable in the setup)

### Start the CoreOS Cluster (single node)
1. Clone the Git repo
1. change directory to vagrant
1. vagrant up

After Vagrant has finished, Jenkins needs some time to startup...

### Port numbers and default username's and password

| App | URL | User | Password |
| --- | --- | ---- | -------- |
| Jenkins | <http://localhost:8080> |      |      |
| Sonar   | <http://localhost:9000> | admin | admin |
| Nexus   | <http://localhost:8081> | admin | admin123 |
| SportsQuest demo app on Tomcat (Test) | <http://localhost:8888/sportsquest> |    |   |
| SportsQuest demo app on Tomcat (Production) | <http://localhost:9999/sportsquest> |    |    |

### Login into the running CoreOS machine
	vagrant ssh
Now you can execute Docker commands. Like show all running containers:
	
	docker ps
	
Or update after Jenkins Docker image changes:

    sudo ./scripts/reload-jenkins.sh

Or copy the Jenkins configuration files over to the shared tmp folder.

    ./scripts/copy-jenkins-config.sh

#### Update after Docker image update
##### By using Vagrant:

	vagrant reload --provision

##### or by using CoreOS:
Stop the relavant service, pull the new docker image and start the service:

	sudo systemctl stop <servicename>
	docker pull <dockerimage>
	sudo systemctl start <servicename>

## Docker container overview

![overview image](https://cdn.rawgit.com/J-Technologies/continuousdelivery/master/images/cd_docker_overview.svg)
Please note:
* The links specified in the image are Docker links using the --link option (http://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/)
* The ordina-cd/* images are build locally and only exists within the CoreOS system.
* Images without a explicit version are implicitly using their latest version

## Jenkins info
Jenkins contains the docker-build-step plugin and is configured to communicate with the docker host running at the CoreOS machine. The Jenkins container is started with a volume parameter that makes the Docker socket available within the Jenkins container.
