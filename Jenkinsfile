pipeline{
  agent any

  environment {
        DOCKER_USERNAME = credentials('docker-hub-username')
        DOCKER_PASSWORD = credentials('docker-hub-password')    
  }
  
  stages{
    stage('Build'){
      steps {
                script {
                    // Construir la imagen de Docker
                    sh 'docker build -t my-app .'
                }
            }
    }
    stage('Push to Docker Registry') {
        steps {
            script {
                // Subir la imagen al Docker Hub (o a otro registry)
                sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                sh 'docker tag my-app $DOCKER_USERNAME/my-app:latest'
                sh 'docker push $DOCKER_USERNAME/my-app:latest'
            }
        }
      }
  }
}
