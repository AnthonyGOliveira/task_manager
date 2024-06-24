pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "task_manager"
        APP_PORT = 80
    }

    stages {
        stage('Preparation') {
            steps {
                script {
                    try {
                        docker.version()
                    } catch (Exception e) {
                        error("Docker CLI not available: ${e.message}")
                    }
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
                    // Definir a tag da imagem Docker com base no branch
                    def DOCKER_TAG = env.GIT_BRANCH ? env.GIT_BRANCH.replaceAll('/', '_') : 'latest'

                    // Construir a imagem Docker usando o Dockerfile no diretório atual
                    def image = docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")

                    // Fazer o push da imagem para o Docker Hub (opcional)
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-cred') {
                        image.push("${DOCKER_TAG}")
                        image.push("latest")
                    }
                }
            }
        }

        stage('Execute') {
            steps {
                script {
                    // Executar a imagem Docker
                    docker.image("${DOCKER_IMAGE}:${env.GIT_BRANCH.replaceAll('/', '_') ?: 'latest'}").run("-p ${APP_PORT}:${APP_PORT}")
                }
            }
        }
    }

    triggers {
        cron('H/5 * * * *')
    }
}
