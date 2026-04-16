pipeline {
      agent any

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
