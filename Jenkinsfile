pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    HEROKU_API_KEY = credentials('heroku-api-key')
    IMAGE_NAME = 'vickyjaura183/jenkin-test-laravel2'
    IMAGE_TAG = 'latest'
    APP_NAME = 'jenkin-test-laravel'
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
    stage('Release the image') {
      steps {
        sh '''
          heroku container:release web --app $APP_NAME
        '''
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}