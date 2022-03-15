pipeline {
  agent any
  environment {
    imageName = 'jaimesalas/my-api-app:latest'
  }
  stages {
    stage('Install dependencies') {
      agent {
        docker {
          image 'node:14-alpine'
          reuseNode true
        }
      }
      steps {
        sh 'npm ci'
      }
    }

    stage('tests') {
      agent {
        docker {
          image 'node:14-alpine'
          reuseNode true
        }
      }
      steps {
        sh 'npm test'
      }
    }

    stage('Build image & push it to DockerHub') {
      when {
        branch 'develop'
      }
      steps {
        script {
          def dockerImage = docker.build(imageName)
          withDockerRegistry([credentialsId: 'dockerhub-credentials', url: '']) {
            dockerImage.push()
            sh 'docker rmi $imageName'
          }
        }
      }
    }
    stage('Deploy to server') {
      when {
        branch 'develop'
      }
      environment {
        containerName = 'my-api-app'
        ec2Instance = 'ec2-user@ec2-13-36-172-245.eu-west-3.compute.amazonaws.com'
        appPort = 80
      }
    }
  }
}