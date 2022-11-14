pipeline{
    agent none
    options{
        buildDiscarder(logRotator(numToKeepStr:'5'))
    }
    environment{
        HEROKU_TEST_API = credentials('heroku-test-api')
        IMAGE_NAME = 'cloand/jenkins-example-react'
        IMAGE_TAG = 'latest'
        APP_NAME =  'react-app-2011'
    }
    stages{
        stage('build'){
            steps{
                sh 'docker build -t $IMAGENAME:$IMAGETAG .'
            }
        }
        stage('login'){
            steps{
                sh 'scho $HEROKU_API_KEY | --username=_--password-stdin registry.heroku.com'
            }
        }
        stage('push heroku to registry'){
            steps{
                sh '''
                    docker tag $IMAGE_NAME:$IMAGE_TAG registry.heroku.com/$APP_NAME/web
                    docker push registry.heroku.com/$APP_NAME/web
                '''
            }
        }
        stage('release the image'){
            steps{
                sh '''
                    heroku container:release web --app=$APP_NAME
                '''
            }
        }
    }
    post{
        always{
            sh 'docker logout'
        }
    }
}
