pipeline {
    agent any

    environment {
        // Definir variáveis de ambiente globais
        DOCKER_IMAGE = 'task_manager'
        DOCKER_IMAGE_VERSION = "1.0.0"
        APP_PORT = 80
        LOCAL_PORT = 8085
    }

    stages {
        stage('Cleanup') {
            steps {
                script {
                    // Remover containers da imagem antiga, se existirem
                    sh "docker ps -a | grep '${DOCKER_IMAGE}:${DOCKER_IMAGE_VERSION}' | awk '{print \$1}' | xargs -r docker rm -f || true"

                    // Remover imagens anteriores com a mesma tag
                    sh "docker images '${DOCKER_IMAGE}:${DOCKER_IMAGE_VERSION}' | awk '{print \$3}' | xargs -r docker rmi -f || true"
                }
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Construir a imagem Docker usando o Dockerfile no diretório atual
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_IMAGE_VERSION} ."
                }
            }
        }

        stage('Execute') {
            steps {
                script {
                    // Executar a imagem Docker
                    sh "docker run -d -p ${LOCAL_PORT}:${APP_PORT} ${DOCKER_IMAGE}:${DOCKER_IMAGE_VERSION}"
                }
            }
        }
    }
}
