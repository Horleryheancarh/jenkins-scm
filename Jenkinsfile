pipeline {
  agent any

  stages {
    stage('Connect to Gihub') {
      steps {
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Horleryheancarh/jenkins-scm.git']])
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          sh 'docker build -t dockerfile'
        }
      }
    }

    stage('Run Docker Container') {
      steps {
        script {
          sh 'docker run -itd -p 8081:80 dockerfile'
        }
      }
    }
  }
}