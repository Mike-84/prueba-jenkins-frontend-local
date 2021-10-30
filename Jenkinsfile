
def version = '1.0.0'

pipeline {
    agent {
        label 'master'
    }

    stages {
        stage('Instalar dependencias'){
            steps {
                script {
                    sh '''
                    cd frontend
                    npm install
                    '''
                }
            }
        }

        stage('Ejecutar linters'){
            steps {
                script {
                    sh '''
                    cd frontend
                    npm run lint
                    '''
                }
            }
        }

        stage ('tests'){
            parallel {
              stage('test unitario') {
                  steps {
                      script {
                          sh '''
                          cd frontend
                          npm run unit
                          '''
                      }

                  }
              }
              stage('test de integración') {
                  steps {
                      script {
                          sh '''
                          cd frontend
                          npm run e2e
                          '''
                      }
                  }
              }
            }
        }

        stage ('contenido estático y nginx (?¿)') {
            steps {
                script {
                    sh '''
                    cd frontend
                    npm run build
                    '''
                }
            }
        }

        stage('Publicar artefacto a nexus') {
            steps{
              script{
                sh "cd frontend"
                sh "docker build --no-cache -t frontend-test:1.0.0 ."
                sh "docker login -u admin -p admin localhost:8082"
                sh "docker image tag frontend-test:1.0.0 localhost:8082/frontend-test:1.0.0"
                sh "docker image push localhost:8082/frontend-test:1.0.0"
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
