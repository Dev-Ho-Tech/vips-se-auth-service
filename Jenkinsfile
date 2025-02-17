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
    stage('Deploy to Remote Server') {
        steps {
            script {
                // Conectarse al servidor remoto mediante SSH
                sh """
                sh 'docker pull lnavarrete14/img-demo:latest'
                sh 'docker ps -q -f name=${DOCKER_CONTAINER} | grep -q . && docker rm -f ${DOCKER_CONTAINER} || echo "No container running"'
                
                // Iniciar el contenedor con la nueva imagen
                sh 'docker run -d --restart always --name ${DOCKER_CONTAINER} -p 7000:7000 $DOCKER_USERNAME/$DOCKER_IMAGE:latest'
                """
            }
        }
    }
  }
}
