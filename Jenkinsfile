pipeline {
  agent any

  environment {
    COMPOSE_PROJECT_NAME = "my_flask_app"
    DOCKERHUB_NAMESPACE = "arjun74"
  }

  stages {

    stage('Clone repo') {
      steps {
        git 'https://your-repo-url.git'
      }
    }

    stage('Build Docker images') {
      steps {
        sh 'docker-compose build'
      }
    }

    stage('Tag images for Docker Hub') {
      steps {
        // Note: docker-compose names images as folder_service
        sh 'docker tag user-service_user-service:latest $DOCKERHUB_NAMESPACE/user-service:latest'
        sh 'docker tag data-service_data-service:latest $DOCKERHUB_NAMESPACE/data-service:latest'
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
        // Optional: If you want to stop containers after test
        // sh 'docker-compose down'
      }
    }
  }
}
