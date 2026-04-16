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

                    bat 'echo %PASSWORD% | docker login -u %USERNAME% -p dckr_pat_U-S1P5GkGGIE1jbsN3KKu4iA4jY' 

                } 

            } 

        }
      }

  }
