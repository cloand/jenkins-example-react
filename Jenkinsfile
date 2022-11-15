pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    HEROKU_API_KEY = credentials('heroku-test-api')
    IMAGE_NAME = 'cloand/jenkins-example-react'
    IMAGE_TAG = 'latest'
    APP_NAME = 'react-app-2012'
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $HEROKU_API_KEY | docker login --username=_ --password-stdin registry.heroku.com'
      }
    }
    stage('Push to Heroku registry') {
      steps {
        sh '''
          docker tag $IMAGE_NAME:$IMAGE_TAG registry.heroku.com/$APP_NAME/web
          docker push registry.heroku.com/$APP_NAME/web
        '''
      }
    }
    stage('Release the image') {
      steps{
      withEnv(['PATH+HEROKU=/opt/homebrew/bin/heroku']) {
            sh '''
              heroku container:release web --app=$APP_NAME
            '''
          }
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
