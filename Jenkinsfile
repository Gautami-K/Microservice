def service = "adservice"

pipeline {
  agent any

  environment {
    SERVICE_NAME = ${service}
  }

  stages {
    stage('Build Docker Image') {
      steps {
        script {
            def imageTag = env.BUILD_NUMBER
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
