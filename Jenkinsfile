pipeline {
    agent any

    environment {
        DOCKER_SERVER = '192.168.64.3'
        IMAGE_NAME = 'docker/welcome-to-docker'
        GIT_REPO = 'https://github.com/Nyamkaann/node-app.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: "${env.GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${env.IMAGE_NAME}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withServer("${env.DOCKER_SERVER}") {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    docker.withServer("${env.DOCKER_SERVER}") {
                        sh "docker run -d -p 8080:8080 --name myapp --rm ${env.IMAGE_NAME}:latest"
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'The build and deployment were successful.'
        }
        failure {
            echo 'The build or deployment failed.'
        }
    }
}

