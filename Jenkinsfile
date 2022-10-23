pipeline {
  agent any

  environment {
    DOCKER_HUB_REPO = "bjwrd/app.py"
    CONTAINER_NAME = "flask-webapp"
  }
  
  stages {
    stage('Checkout') {
           steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/BJWRD/cicd-kubernetes-jenkins-pipeline']]])
            }
    }

    
    stage('Build') {
      steps {
        script {
         echo 'Building Image...'
         sh 'docker image build -t $DOCKER_HUB_REPO:latest .'
        }
      }
    }

    stage('Test') {
        steps {
            echo 'Testing..'
            sh 'docker stop $CONTAINER_NAME || true'
            sh 'docker rm $CONTAINER_NAME || true'
            sh 'docker run -d --name $CONTAINER_NAME $DOCKER_HUB_REPO'
            sh 'docker exec $(docker ps -q -f name=flask-webapp) bash -c "flake8 /opt/app.py"'
        }
    }

    stage('Push') {
      steps {
        script {
          echo 'Pushing Image...'
          withDockerRegistry([ credentialsId: "dockerhublogin", url: "" ]) {
          sh 'docker push $DOCKER_HUB_REPO:latest'
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying Image...'
        sh 'kubectl create -f namespace.yml'
        sh 'kubectl create -f cicd-kubernetes-jenkins-pipeline.yml'
      }
    }
  }
}