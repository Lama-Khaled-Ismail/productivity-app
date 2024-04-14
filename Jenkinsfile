pipeline {
  agent any
  tools{
    nodejs 'node-latest'
  }
  environment {
    MONGODB_URI = credentials('mongodb-uri')
    TOKEN_KEY = credentials('token-key')
    EMAIL = credentials('email')
    PASSWORD = credentials('password')
  }
  stages {
    stage('Checkout') {
      steps {
        git(url: 'https://github.com/Lama-Khaled-Ismail/productivity-app.git', branch: 'main')
      }
    }

    stage('Front-end Test') {
      steps {
        dir(path: 'client') {
          sh 'npm install'
          sh 'npm test'
        }

      }
    }

    stage('Back-end Test'){
      steps {
        dir('server') {
          sh 'npm install'
          sh 'export MONGODB_URI=$MONGODB_URI'
          sh 'export TOKEN_KEY=$TOKEN_KEY'
          sh 'export EMAIL=$EMAIL'
          sh 'export PASSWORD=$PASSWORD'
          sh 'npm test'
        }
      }
    }

    stage('Build Images') {
      steps {
        sh 'docker build -t lamakhaled/productivity-app:client-${BUILD_NUMBER} client'
        sh 'docker build -t lamakhaled/productivity-app:server-${BUILD_NUMBER} server'
      }
    }

    stage('Push Images ') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
        sh 'docker push rakeshpotnuru/productivity-app:client-${BUILD_NUMBER}'
        sh 'docker push rakeshpotnuru/productivity-app:server-${BUILD_NUMBER}'
        }
      }    

    }
}