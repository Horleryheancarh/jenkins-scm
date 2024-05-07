pipeline {
  agent any

  environment {
		DOCKERHUB=credentials('dockerhub')
	}

  stages {
    stage('Connect to Gihub') {
      steps {
        git branch: 'main', url: 'https://github.com/Horleryheancarh/jenkins-scm.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          sh 'docker build -t yheancarh/jenkins-scm .'
        }
      }
    }

    stage('Run Docker Container') {
      steps {
        script {
          // sh 'docker stop test-app'

          sh 'docker run -itd -p 8081:80 yheancarh/jenkins-scm --name test-app'
        }
      }
    }

    stage('Push to Docker') {
      steps {
        script {
          sh 'echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin'

          sh 'docker run -itd -p 8081:80 yheancarh/jenkins-scm'
        }
      }
    }

    stage('Cleanup') {
      steps {
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
        
        sh 'docker logout'
        
        sh 'docker system prune -f'
      }
    }
  }
}