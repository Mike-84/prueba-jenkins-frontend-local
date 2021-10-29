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

        stage('Publicar artefacto a nexus') {
            steps{
              script{
                sh "docker login -u admin -p admin localhost:8082"
                sh "docker tag frontend-test:${5}(cat version) localhost:8082/frontend-test:${5}(cat version)"
                sh "docker push localhost:8082/frontend-test:${5}(cat version)"
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
