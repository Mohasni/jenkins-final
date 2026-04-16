pipeline {
      agent any

      environment {
          DOCKER_IMAGE = 'mohasni/getting-started-app'
          DOCKER_TAG = "${env.BUILD_NUMBER}"
      }

      stages {
          stage('Clone') {
              steps {
                  echo 'Clone du depot Git...'
                  checkout scm
              }
          }

          stage('Build Docker Image') {
              steps {
                  bat script: """
                      docker build -t ${env.DOCKER_IMAGE}:${env.DOCKER_TAG} .
                      docker tag ${env.DOCKER_IMAGE}:${env.DOCKER_TAG} ${env.DOCKER_IMAGE}:latest
                  """
              }
          }

          stage('Push to Docker Hub') {
              steps {
                  withCredentials([usernamePassword(
                      credentialsId: 'docker-hub-credentials',
                      usernameVariable: 'DOCKER_USER',
                      passwordVariable: 'DOCKER_PASS'
                  )]) {
                      bat script: """
                          docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                          docker push %DOCKER_IMAGE%:%DOCKER_TAG%
                          docker push %DOCKER_IMAGE%:latest
                      """, label: 'Push to Docker Hub'
                  }
              }
          }

          stage('Deploy') {
              steps {
                  bat "docker-compose up -d"
              }
          }
      }

      post {
          always {
              bat "docker-compose down -v 2>NUL"
              bat "docker rmi %DOCKER_IMAGE%:%DOCKER_TAG% %DOCKER_IMAGE%:latest 2>NUL"
              cleanWs()
          }
      }
  }
