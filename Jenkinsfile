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
                    sh "docker build --no-cache -t frontend-test:${version}"
                }
            }
        }

        stage('Publicar artefacto a nexus') {
            steps{
              script{
                sh "docker login -u admin -p admin localhost:8082"
                sh "docker tag frontend-test:${version} localhost:8082/frontend-test:${version}"
                sh "docker push localhost:8082/frontend-test:${version}"
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
