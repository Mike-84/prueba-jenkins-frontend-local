pipeline {
    agent {
        label 'master'
    }

    stages {
        stage('Instalar dependencias'){
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Ejecutar linters'){
            steps {
                script {
                    sh 'npm run lint'
                }
            }
        }

        stage ('tests'){
            parallel {
              stage('test unitario') {
                  steps {
                      script {
                          sh 'npm run unit'
                      }

                  }
              }
              stage('test de integración') {
                  steps {
                      script {
                          sh 'npm run e2e'
                      }
                  }
              }
            }
        }

        stage('Passwords') {
          parallel {
            stage(Pass-nexus) {
              steps {
                  script{
                      sh 'docker exec prueba-jenkins-frontend-local_nexus_1 cat /nexus-data/admin.password'
                  }
              }
            }
            stage(Pass-jenkins) {
              steps {
                  script{
                      sh 'docker exec -u root prueba-jenkins-frontend-local_jenkins_1 cat /var/jenkins_home/secrets/initialAdminPassword'
                  }
              }
            }
          }
        }

        stage('Publicar artefacto a nexus') {
            steps{
              script{
                sh "docker login -u admin -p admin localhost:8082"
                sh "docker tag frontend-test:$(cat version) localhost:8082/frontend-test:$(cat version)"
                sh "docker push localhost:8082/frontend-test:$(cat version)"
              }
            }
        }

    }

    post {
        cleanup {
            cleanWs()
        }
    }
}
