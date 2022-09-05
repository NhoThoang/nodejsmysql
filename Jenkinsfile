pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKER_CREDENTIALS = credentials('nodejsapp')
    apinodejsTag    = $GIT_TAG_NAME 
  }
  stages {
    stage('Build') {
      steps {
        sh 'whoami && pwd'
        sh 'docker build -t registry.gitlab.com/xzhoang/nodejsmysql:$apinodejsTag  . '
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKER_CREDENTIALS_PSW'
	sh 'echo Git_Tag_Name= $apinodejsTag'
        sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR registry.gitlab.com/xzhoang/nodejsmysql   --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push registry.gitlab.com/xzhoang/nodejsmysql:$apinodejsTag '
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}

