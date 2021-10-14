pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('tg5688-dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build --compress -t tg5688/k8s-goalsapp-frontend:v2 ./frontend'
        sh 'docker build --compress -t tg5688/k8s-goalsapp-backend:v2 ./backend'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'echo "Push frontend."'
        sh 'docker push tg5688/k8s-goalsapp-frontend:v2'
        sh 'docker tag tg5688/k8s-goalsapp-frontend:v2 k8s-goalsapp-frontend:latest'
        sh 'docker push tg5688/k8s-goalsapp-frontend:latest'

        sh 'echo "Push backend."'
        sh 'docker push tg5688/k8s-goalsapp-backend:v2'
        sh 'docker tag tg5688/k8s-goalsapp-backend:v2 k8s-goalsapp-backend:latest'
        sh 'docker push tg5688/k8s-goalsapp-backend:latest'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}