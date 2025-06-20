pipeline {
  agent any

  tools {
    maven 'maven-3' // Make sure 'maven-3' is configured in Jenkins -> Global Tool Configuration
  }

  environment {
    IMAGE_NAME = "saikiranbiradar/helloapp:${BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Biradar09/gitops-java-demo.git'
      }
    }

    stage('Build with Maven') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') { // 'SonarQube' must match the name configured in Jenkins -> Configure System
          sh 'mvn sonar:sonar'
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${IMAGE_NAME}")
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh """
            echo $PASS | docker login -u $USER --password-stdin
            docker push ${IMAGE_NAME}
          """
        }
      }
    }
  }
}
