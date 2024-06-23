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

                    // Instalar plugin Docker Pipeline
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-cred', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        sh 'docker plugin install docker-latest'
                    }

                    // Construir a imagem Docker usando o Dockerfile no diretório atual
                    docker.build(imageName: "${DOCKER_IMAGE}:${GIT_BRANCH}", context: '.')

                    // Fazer o push da imagem para o Docker Hub (opcional)
                    // docker.withRegistry('https://index.docker.io', 'docker-hub-cred') {
                    //     push "${DOCKER_IMAGE}:${GIT_BRANCH}"
                    // }
                }
            }
        }

        stage('Execute') {
            steps {
                script {
                    // Definir variável de ambiente para a porta da aplicação
                    def APP_PORT = 80

                    // Executar a imagem Docker
                    docker.run(image: "${DOCKER_IMAGE}:${env.GIT_BRANCH}", ports: [APP_PORT:"${APP_PORT}"])
                }
            }
        }
    }
}
