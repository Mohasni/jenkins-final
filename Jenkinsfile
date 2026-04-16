pipeline {
      agent any

      environment {
          DOCKER_IMAGE = 'mohasni/getting-started-app'
          DOCKER_TAG = "${env.BUILD_NUMBER}"
      }

      stages {
stage('Login to Docker Hub') { 

            steps { 

                withCredentials([usernamePassword( 

                credentialsId: 'docker-credentials', 

                usernameVariable: 'USERNAME', 

                passwordVariable: 'PASSWORD' 

                )]) { 

                    bat 'echo %PASSWORD% | docker login -u $USERNAME --password-stdin' 

                } 

            } 

        }
      }

  }
