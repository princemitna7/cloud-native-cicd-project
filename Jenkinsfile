pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "princemitna/devops-project:latest"
  }

  stages {

    stage('Clone Repo') {
      steps {
        git branch: 'main',
          url: 'https://github.com/princemitna7/devops-project.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build --network=host -t $DOCKER_IMAGE app/'
      }
    }

    stage('Push Docker Image') {
      steps {
        withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
          sh """
          echo $PASS | docker login -u princemitna --password-stdin
          docker push $DOCKER_IMAGE
          """
        }
      }
    }

    stage('Deploy using Ansible') {
      steps {
        sh 'ansible-playbook ansible/deploy.yml'
      }
    }
  }
}

