pipeline{
  agent any

  environment {
        DOCKER_IMAGE = 'img-demo'
        DOCKER_CONTAINER = 'se-demo'
        DOCKER_USERNAME = credentials('docker-hub-username')
        DOCKER_PASSWORD = credentials('docker-hub-password')    
  }
  
  stages{
    stage('Build'){
      steps {
                script {
                    // Construir la imagen de Docker
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
    }
    stage('Push to Docker Registry') {
        steps {
            script {
                // Subir la imagen al Docker Hub (o a otro registry)
                sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                sh 'docker tag my-app $DOCKER_USERNAME/$DOCKER_IMAGE:latest'
                sh 'docker push $DOCKER_USERNAME/$DOCKER_IMAGE:latest'
                
            }
        }
    }
    stage('Deploy to Remote Server1111') {
        steps {
            script {
                sh 'docker pull $DOCKER_USERNAME/$DOCKER_IMAGE:latest'
            }
        }
    }
  }
}
