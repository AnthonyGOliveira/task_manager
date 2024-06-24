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
                    def DOCKER_IMAGE = "task_manager"
                    def GIT_BRANCH = env.GIT_BRANCH ?: 'latest'
                    def DOCKER_IMAGE_VERSION = "1.0.0"

                    // Construir a imagem Docker usando o Dockerfile no diretório atual
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_IMAGE_VERSION} ."
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
                    sh "docker run -p ${LOCAL_PORT}:${APP_PORT} ${DOCKER_IMAGE}:${DOCKER_IMAGE_VERSION}"
                }
            }
        }
    }
}
