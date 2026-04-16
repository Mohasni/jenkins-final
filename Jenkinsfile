pipeline {
      agent any

      environment {
          DOCKER_IMAGE = 'VOTRE_DOCKER_ID/getting-started-app'
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
                  script {
                      def image = docker.build("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}")
                      image.tag('latest')
                  }
              }
          }

          stage('Push to Docker Hub') {
              steps {
                  script {
                      withCredentials([usernamePassword(
                          credentialsId: 'docker-hub-credentials',
                          usernameVariable: 'DOCKER_USER',
                          passwordVariable: 'DOCKER_PASS'
                      )]) {
                          sh '''
                              docker login -u $DOCKER_USER -p $DOCKER_PASS
                              docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                              docker push ${DOCKER_IMAGE}:latest
                          '''
                      }
                  }
              }
          }

          stage('Deploy') {
              steps {
                  sh 'docker-compose up -d'
              }
          }
      }

      post {
          always {
              sh 'docker-compose down -v 2>/dev/null || true'
              sh 'docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest 2>/dev/null || true'
              cleanWs()
          }
      }
  }