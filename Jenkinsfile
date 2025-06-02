def IMAGE_TAG = env.BUILD_NUMBER

pipeline {
  agent any

  environment {
    SERVICE_NAME = 'currencyservice'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        script {
            withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
              sh "docker build -t gautamiii/${SERVICE_NAME}:${IMAGE_TAG} ."
            }
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
            withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
              sh "docker push gautamiii/${SERVICE_NAME}:${IMAGE_TAG}"
            }
        }
      }
    }

    stage('Trigger Deployment') {
      steps {
        build job: 'main', parameters: [
          string(name: 'SERVICE_NAME', value: "${SERVICE_NAME}"),
          string(name: 'IMAGE_TAG', value: "${IMAGE_TAG}")
        ]
      }
    }
  }
}
