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

        stage('Publicar artefacto a nexus') {
            steps{
              script{
                sh '''
                docker login -u admin -p admin localhost:8082
                docker image tag frontend-test:${version} localhost:8082/frontend-test:${version}
                docker image push localhost:8082/frontend-test:${version}
                '''
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
