pipeline {
  agent {
    docker {
      image 'node:15-alpine'
      args '-p 3000:3000'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      environment {
        CI = 'true'
      }
      steps {
        sh './jenkins/scripts/test.sh'
      }
    }

    stage('error') {
      parallel {
        stage('error') {
          steps {
            sh './jenkins/scripts/deliver.sh'
            input 'inished using the web site? (Click "Proceed" to continue)'
            sh './jenkins/scripts/kill.sh'
          }
        }

        stage('Add \'Deliver\' stage') {
          steps {
            sh './jenkins/scripts/deliver.sh'
            input 'Finished using the web site? (Click "Proceed" to continue)'
            sh './jenkins/scripts/kill.sh'
          }
        }

      }
    }

  }
}