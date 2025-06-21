stage('Push to Docker Hub') {
  steps {
    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
      sh """
        echo \$PASS | docker login -u \$USER --password-stdin
        docker push \$IMAGE_NAME
      """
    }
  }
}
