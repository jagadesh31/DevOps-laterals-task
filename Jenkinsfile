pipeline {
  agent any

  environment {
    DOCKER_USER = "jagadesh31"
    TAG = "${BUILD_NUMBER}"
  }

  stages {

    stage('Build Images') {
      steps {
        sh '''
          docker build -t $DOCKER_USER/Frontend:$TAG frontend
          docker build -t $DOCKER_USER/Backend:$TAG backend
          docker build -t $DOCKER_USER/nginx:$TAG nginx
        '''
      }
    }

    stage('Push Images') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'USER',
          passwordVariable: 'PASS'
        )]) {
          sh '''
            echo "$PASS" | docker login -u "$USER" --password-stdin
            docker push $DOCKER_USER/frontend:$TAG
            docker push $DOCKER_USER/backend:$TAG
            docker push $DOCKER_USER/nginx:$TAG
          '''
        }
      }
    }

    stage('Deploy') {
      steps {
        sh '''
          export TAG=$TAG
          docker compose pull
          docker compose up -d
        '''
      }
    }
  }
}
