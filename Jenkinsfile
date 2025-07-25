pipeline {
  agent any

  environment {
    COMPOSE_PROJECT_NAME = "my_flask_app"
  }

  stages {

    stage('Clone repo') {
      steps {
        git branch: 'main', url: 'https://github.com/arjun7976/devops-microservices-main.git'
      }
    }

    stage('Build Docker images') {
      steps {
        sh 'docker-compose build'
      }
    }

    stage('Run containers') {
      steps {
        sh 'docker-compose up -d'
      }
    }

    stage('Check services') {
      steps {
        sh 'docker-compose ps'
      }
    }

    stage('Test endpoints') {
      steps {
        sh 'curl -f http://localhost:5000/ || exit 1'
        sh 'curl -f http://localhost:5001/user/alice || exit 1'
      }
    }

    stage('Clean up') {
      steps {
        sh 'docker-compose down'
      }
    }
  }
}
