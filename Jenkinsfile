pipeline {
  agent any
  stages {
    stage('Install dependencies') {
      agent {
        docker {
          image 'node:14-alpine'
        }
      }
      steps {
        sh 'npm ci'
      }
    }

    stage('build') {
      agent {
        docker {
          image 'node:14-alpine'
          reuseNode true
        }
      }
      steps {
        sh 'npm run build'
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
  }
}