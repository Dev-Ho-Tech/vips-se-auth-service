pipeline{
  agent any

  environment {
        DOCKER_IMAGE = 'img-demo'
        DOCKER_CONTAINER = 'se-demo'
        DOCKER_USERNAME = credentials('docker-hub-username')
        DOCKER_PASSWORD = credentials('docker-hub-password')    
  }
  
  stages{
    stage('Builds'){
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
    stage('Deploy to Remote Server') {
        steps {
            script {
                sh 'docker pull $DOCKER_USERNAME/$DOCKER_IMAGE:latest'
                sh 'docker ps -q -f name=${DOCKER_CONTAINER} | grep -q . && docker rm -f ${DOCKER_CONTAINER} || echo "No container running"'
                sh 'docker run -d --restart always --name ${DOCKER_CONTAINER} -p 7000:7000 $DOCKER_USERNAME/$DOCKER_IMAGE:latest'
            }
        }
    }
    stage('Clean Up') {
        steps {
            script {
                // Limpiar imágenes no utilizadas para evitar que el espacio se llene
                sh 'docker system prune -f --volumes'
            }
        }
    }
  }
  post {
        always {
            // Enviar notificación al finalizar
            echo "Pipeline finished"
        }
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Deployment failed!"
            // Puedes agregar un paso para notificar a Slack o correo aquí.
        }
  }
}
