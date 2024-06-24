pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "task_manager"
        APP_PORT = 80
    }

    stages {
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
                    def dockerImage = docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")

                    // Fazer o push da imagem para o Docker Hub (opcional)
                    dockerImage.push()
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
