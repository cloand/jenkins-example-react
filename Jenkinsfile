pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    HEROKU_API_KEY = credentials('heroku-test-api')
    IMAGE_NAME = 'cloand/jenkins-example-react'
    IMAGE_TAG = 'latest'
    APP_NAME = 'resct-app-2011'
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build --privileged -t $IMAGE_NAME:$IMAGE_TAG .'
      }
    }
   
  }
}
