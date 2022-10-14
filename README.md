# cicd-kubernetes-jenkins-pipeline
Within this project we will be building and deploying a [Python Flask](http://flask.pocoo.org/) web application using a [Jenkins](https://www.jenkins.io/) CICD Pipeline, [Kubernetes](https://kubernetes.io/) as the container orchestrator, [Docker](https://www.docker.com/) as the application host and [NodePort](https://kubernetes.io/docs/concepts/services-networking/service/) as a service to ensure the application is externally accessible. 

## Architecture
This K8s architecture consists of one Node and one Pods using Deployment and the NodePort service for external site accessibility. The Node will reside within the Namespace `flask-namespace` and the bulk of the configuration can be found within the `kubernetes-docker-python-flask.yml`.

<img width="627" alt="image" src="https://user-images.githubusercontent.com/83971386/194745323-024095ca-8e8a-4f27-9482-ae5905dd556f.png">

## Prerequisites
* Minikube installation - [steps](https://minikube.sigs.k8s.io/docs/start/)
* Virtualbox installation - [steps](https://www.virtualbox.org/wiki/Downloads)
* Docker installation - [steps](https://docs.docker.com/engine/install/)

## Build Process
This section details the steps required to deploy the Python Flask Web application via Kubernetes/Docker using a Jenkins CI/CD Pipeline.

## Install Jenkins
###   1. Run Jenkins Container
Before we begin the Jenkins Installation, we need to ensure that docker has been installed on the VM you are using. Please follow the steps within the 'Prerequisites - Docker Installation' section to get started.

Once Docker has been installed, execute the following Docker run command to start out Jenkins container in detatched mode. This will host our Jenkins Pipeline.

      docker run -u 0 --privileged --name jenkins -it -d -p 8080:8080 -p 5000:5000 \
      -v /var/run/docker.sock:/var/run/docker.sock \
      -v $(which docker):/usr/bin/docker \
      -v /home/jenkins_home:/var/jenkins_home \
      jenkins/jenkins:latest
      
###   2. Unlocking Jenkins
After running the container you should be able to access the Jenkins application via web browser using ```http://localhost:8080``` or ```http://<host_ip>:8080```.

Initially you will notice that you are presented with a 'Unlock Jenkins' screen. To retrieve the requested 'Administrator password' you will need to enter the following docker command below to view the container logs and locate the password -
      
      docker logs <containerID>

Example:

<img width="656" alt="image" src="https://user-images.githubusercontent.com/83971386/195887709-16190167-11f1-405a-adf5-6e2537b0d7ae.png">

Once retrieved, copy and paste the password into the 'Administrator password' field -

<img width="848" alt="image" src="https://user-images.githubusercontent.com/83971386/195887952-7930b373-175c-4d99-81d6-31187fc86807.png">

###   3. Customize Jenkins
Select 'Install suggested plugins' and wait for the completed installation -

<img width="871" alt="image" src="https://user-images.githubusercontent.com/83971386/195888092-df15273c-bb37-4534-8af5-05bea6a46e3e.png">

Note: In the instance all of the plugins fail, you may need to enter the following commands to ensure a HTTP connection is established rather than HTTPS when pulling the Jenkins plugins -

      docker exec -it <containerID> bash
      sed -i 's/https/http/g' /var/jenkins_home/hudson.model.UpdateCenter.xml 
 
 Example:
 
<img width="704" alt="image" src="https://user-images.githubusercontent.com/83971386/195888177-aad8e0a2-8aa5-41ed-b440-6a039e70244f.png">

###   4. Creating Jenkins Admin User
You will then be presented with the following 'Create First Admin User' screen, enter details relevant to yourself and select 'Continue'.

<img width="781" alt="image" src="https://user-images.githubusercontent.com/83971386/195888296-ab95b2c2-dfce-4dc2-b50d-69b861c9bffe.png">

## Install Docker Pipeline Plugin 
From the Jenkins Dashboard, select the 'Manage Jenkins' option on the left-hand side, followed by 'Manage Plugins' and then the 'Available Plugins' widget.

Within the Search field, enter 'Docker Pipeline' and select the 'Install without restart'button -

<img width="731" alt="image" src="https://user-images.githubusercontent.com/83971386/195888404-2d7a605d-8bec-4e0b-9c3c-819ddcbf55b1.png">

<img width="371" alt="image" src="https://user-images.githubusercontent.com/83971386/195888471-8d6fcb01-742b-46cd-8bdb-f3549ee1b3d9.png">


## Create a Jenkins pipeline

      
## How to Apply/Destroy
This section details the deployment and teardown of the cicd-kubernetes-jenkins-pipeline architecture. 

## Deploy Kubernetes using Jenkins Pipeline



## List of tools/services used
* [Jenkins](https://www.jenkins.io/)
* [Docker](https://www.docker.com/)
* [Dockerfile](https://docs.docker.com/engine/reference/builder/)
* [Docker Hub](https://hub.docker.com/)
* [Minikube](https://minikube.sigs.k8s.io/docs/)
* [K8s Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [Services- NodePort](https://kubernetes.io/docs/concepts/services-networking/service/)
* [Draw.io](https://www.draw.io/index.html)
