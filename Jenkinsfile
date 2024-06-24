pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Definir variáveis de ambiente
                    def DOCKER_IMAGE = "task_manager_1.0.0"
                    def GIT_BRANCH = env.GIT_BRANCH

                    // Construir a imagem Docker usando o Dockerfile no diretório atual
                    sh "docker build -t ${DOCKER_IMAGE}:${GIT_BRANCH} ."
                }
            }
        }

        stage('Execute') {
            steps {
                script {
                    // Definir variável de ambiente para a porta da aplicação
                    def APP_PORT = 80
                    def LOCAL_PORT = 8085

                    // Executar a imagem Docker
                    sh "docker run -p ${LOCAL_PORT}:${APP_PORT} ${DOCKER_IMAGE}:${env.GIT_BRANCH}"
                }
            }
        }
    }
}
