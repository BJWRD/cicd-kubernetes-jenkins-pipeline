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

## Adding Credentials
###   1. Adding Docker Hub Credentials
Before we begin with the Pipeline creation, we will need to add our Docker Hub and Git credentials to our Jenkins profile.

Select 'Manage Jenkins' -

<img width="199" alt="image" src="https://user-images.githubusercontent.com/83971386/195980344-fe6f3028-c6b8-4cb1-860e-ab23c5c40356.png">

Followed by 'Manage Credentials' - 

<img width="636" alt="image" src="https://user-images.githubusercontent.com/83971386/195980368-bb7bb47c-8cd3-4206-a55e-cd642a133372.png">

Select the 'Global' hyperlink -

<img width="460" alt="image" src="https://user-images.githubusercontent.com/83971386/195980385-58b72a3f-ef3a-41cc-b18d-4b3786f5d5ce.png">

And then click on 'Add Credentials', from here you can populate the following screen with your Docker Hub login credentials and save -

<img width="1213" alt="image" src="https://user-images.githubusercontent.com/83971386/195980417-df538493-b935-4331-9533-cdac898a1be7.png">

Example:
<img width="1218" alt="image" src="https://user-images.githubusercontent.com/83971386/195980445-ae882dca-9f39-4c1f-8149-87f4194be0fb.png">

###   2. Adding Git Credentials
Repeat the steps above, and enter your Git related details. See below -

ENTER IMAGE GIT

## Create a Jenkins pipeline
Within the Jenkins Dashboard select the 'New Item' option on the left-hand side, followed by 'Create a Job' -

ENTER IMAGE

You will then be presented with multiple items which can be created. We will need to enter an item name, followed by the Pipeline selection -

ENTER IMAGE

Scroll down to the 'Pipeline' section and select the following Pipeline definition and copy and paste your Jenkinsfile contents within the Script field -

<img width="957" alt="image" src="https://user-images.githubusercontent.com/83971386/195980578-6fcb2033-6a24-4357-b6bd-0087330cedea.png">

Click 'Save' with the 'Groovy Sandbox' tickbox selected.

NOTE: if your Jenkinsfile exists within your GitHub repo, you can also select the following SCM definition which saves you from copying and pasting the contents within the 'Pipeline Script' field -

<img width="854" alt="image" src="https://user-images.githubusercontent.com/83971386/195980550-5e835589-94f9-4db0-b1a8-3ddcb1814b5e.png">

## Deploy Kubernetes using Jenkins Pipeline
Now we have a created Pipeline, we can finally select 'Build Now' to set the Pipeline build process in motion -

<img width="299" alt="image" src="https://user-images.githubusercontent.com/83971386/195980605-784997cc-ad2d-4951-bfd4-15ac2b798619.png">


## List of tools/services used
* [Jenkins](https://www.jenkins.io/)
* [Docker](https://www.docker.com/)
* [Dockerfile](https://docs.docker.com/engine/reference/builder/)
* [Docker Hub](https://hub.docker.com/)
* [Minikube](https://minikube.sigs.k8s.io/docs/)
* [K8s Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [Services- NodePort](https://kubernetes.io/docs/concepts/services-networking/service/)
* [Draw.io](https://www.draw.io/index.html)
