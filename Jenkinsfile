pipeline {
   agent any

   tools {
      maven 'Maven 3.6.3' // Define the Maven version installed on your Jenkins server
   }

   stages {
      stage('Build') {
         steps {
            script {
               // Build the Docker image
               docker.build("my-java-app:${env.BUILD_ID}")
            }
         }
      }
      stage('Test') {
         steps {
            script {
               // Run tests inside the Docker container
               docker.image("my-java-app:${env.BUILD_ID}").inside {
                  sh 'mvn test'
               }
            }
         }
      }
      stage('Push') {
         steps {
            script {
               // Push the Docker image to Docker Hub
               docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                  docker.image("my-java-app:${env.BUILD_ID}").push('latest')
               }
            }
         }
      }
      stage('Deploy') {
         steps {
            script {
               // Deploy the Docker image to the production environment
               sh 'docker run -d -p 8080:8080 my-java-app:latest'
            }
         }
      }
   }
   post {
      always {
         cleanWs()
      }
   }
}