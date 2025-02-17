pipeline{
  agent any
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
                sh 'docker login -u lnavarrete14 -p Fernanda1731'
                sh 'docker tag my-app lnavarrete14/my-app:latest'
                sh 'docker push lnavarrete14/my-app:latest'
            }
        }
      }
  }
}
