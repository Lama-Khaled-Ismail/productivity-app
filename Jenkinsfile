pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git(url: 'https://github.com/Lama-Khaled-Ismail/productivity-app.git', branch: 'main')
      }
    }

    stage('Front-end Test') {
      steps {
        dir(path: 'client') {
          sh '''npm install
npm test'''
        }

      }
    }

  }
}