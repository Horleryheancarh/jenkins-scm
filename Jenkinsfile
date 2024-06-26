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
          sh 'docker build -t yheancarh/jenkins-scm:${BUILD_NUMBER} .'
        }
      }
    }

    stage('Stop Docker Container') {
      steps {
        script {
          sh 'docker stop test-app'

          sh 'docker rm test-app'
        }
      }
    }

    stage('Run Docker Container') {
      steps {
        script {
          sh 'docker run -itd -p 8081:80 --name test-app yheancarh/jenkins-scm:${BUILD_NUMBER}'
        }
      }
    }

    stage('Push to Docker') {
      steps {
        script {
          sh 'echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin'

          sh 'docker push yheancarh/jenkins-scm:${BUILD_NUMBER}'
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