pipeline {
  agent any

  environment {
    MONGO_URI = credentials('mongo-uri')
  }

  tools {
    nodejs 'NodeJS 16'
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/yourusername/devsecops-demo.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Security Scan') {
      steps {
        sh 'npx audit-ci --moderate'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t devsecops-app .'
      }
    }

    stage('Docker Run') {
      steps {
        sh 'docker run -d -p 3000:3000 --env MONGO_URI=$MONGO_URI devsecops-app'
      }
    }
  }

  post {
    failure {
      echo 'Pipeline failed!'
    }
  }
}
