pipeline {
  agent any

  parameters {
    string(name: 'SERVICE_NAME', defaultValue: '', description: 'Name of the microservice to deploy')
    string(name: 'IMAGE_TAG', defaultValue: '', description: 'Docker image tag to deploy')
  }

  environment {
    DEPLOYMENT_FILE = 'deployment-service.yml'
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

    stage('Update YAML Image Tag') {
      steps {
        script {
          def image = "gautamiii/${params.SERVICE_NAME}:${params.IMAGE_TAG}"
          echo "Updating image for ${params.SERVICE_NAME} to ${image}"

          // Replace only for the specified service
          sh """
            sed -i '/name: ${params.SERVICE_NAME}/,/image:/s|image:.*|image: ${image}|' ${env.DEPLOYMENT_FILE}
          """
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        withKubeCredentials(kubectlCredentials: [[
          caCertificate: '',
          clusterName: 'devopsshack-cluster',
          contextName: '',
          credentialsId: 'k8-token',
          namespace: 'webapps',
          serverUrl: 'https://08071D4F6FC1EB1CC80116A05A71C5DE.gr7.ap-south-1.eks.amazonaws.com'
        ]]) {
          script {
            echo "Applying full deployment file..."
            sh "kubectl apply -f ${env.DEPLOYMENT_FILE} -n webapps"
          }
        }
      }
    }
  }
}
