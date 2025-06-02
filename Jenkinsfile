pipeline {
  agent any

  parameters {
    string(name: 'SERVICE_NAME', defaultValue: '', description: 'Name of the microservice to deploy')
    string(name: 'IMAGE_TAG', defaultValue: '', description: 'Docker image tag to deploy')
  }

  stages {
    stage('Validate Inputs') {
      steps {
        script {
          if (!params.SERVICE_NAME || !params.IMAGE_TAG) {
            error "SERVICE_NAME and IMAGE_TAG must be provided"
          }
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'devopsshack-cluster', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://08071D4F6FC1EB1CC80116A05A71C5DE.gr7.ap-south-1.eks.amazonaws.com']]) {
          script {
            def image = "gautamiii/${params.SERVICE_NAME}:${params.IMAGE_TAG}"

            echo "Deploying ${params.SERVICE_NAME} with image ${image}"

            sh """
              kubectl set image deployment/${params.SERVICE_NAME} ${params.SERVICE_NAME}=${image} -n webapps
            """
          }
        }
      }
    }
  }
}
