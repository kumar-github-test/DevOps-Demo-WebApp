pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building'
        build 'Build Web App'
      }
    }

    stage('Test') {
      steps {
        echo 'Testing'
      }
    }

    stage('Deloy') {
      steps {
        echo 'Deploying'
      }
    }

  }
}