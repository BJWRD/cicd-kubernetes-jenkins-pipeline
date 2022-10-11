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
