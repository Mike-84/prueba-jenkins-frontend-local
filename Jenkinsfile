pipeline {
    agent {
        label 'master'
    }

    stages {
        stage('prueba') {
            steps {
                echo 'Hola mundo'
            }
        }


        stage('netstat'){
            steps {
                script {
                    sh 'netstat -atup'
                }
            }
        }

        stage (frontend){
            parallel {
              stage('npm-build') {
                  steps {
                      echo "la versión de npm es:"
                      script {
                          sh 'npm --version'
                      }

                  }
              }


              stage('prueba-docker') {
                  steps {
                      echo "la versión de docker es:"
                      script {
                          sh 'docker --version'
                      }

                  }
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
