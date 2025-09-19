pipeline {
  agent any
  environment {
    IMAGE_NAME = "devsecops-demo"
    IMAGE_TAG  = "${env.BUILD_NUMBER}"
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build Docker image') {
      steps {
        sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
      }
    }
    stage('Scan image with Trivy') {
      steps {
        sh '''
          docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
            ghcr.io/aquasecurity/trivy:latest \
            image --exit-code 1 --severity HIGH,CRITICAL ${IMAGE_NAME}:${IMAGE_TAG}
        '''
      }
    }
  }
}
