pipeline {
  agent any

  environment {
    COMPOSE_PROJECT_NAME = "my_flask_app"
    DOCKERHUB_NAMESPACE = "arjun79"  // <<--- CHANGE THIS
  }

  stages {

    stage('Clone repo') {
      steps {
        git 'https://github.com/arjun7976/devops-microservices-main.git'
      }
    }

    stage('Build Docker images') {
      steps {
        sh 'docker-compose build'
      }
    }

    stage('Tag images') {
      steps {
        sh 'docker tag devops-microservices-main_user-service:latest $DOCKERHUB_NAMESPACE/user-service:latest'
        sh 'docker tag devops-microservices-main_data-service:latest $DOCKERHUB_NAMESPACE/data-service:latest'
      }
    }

    stage('Push images to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
          sh 'docker push $DOCKERHUB_NAMESPACE/user-service:latest'
          sh 'docker push $DOCKERHUB_NAMESPACE/data-service:latest'
        }
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
