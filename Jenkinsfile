pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKER_IMAGE = "fidonoso/my-spring-boot-app"
        REGISTRY = "hub.docker.com" // e.g., Docker Hub or any other registry
    }

    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio
                git branch: 'main', url: 'https://github.com/fidonoso/BOT-my-spring-boot-app'
            }
        }
        stage('Build') {
                agent {
                    docker {
                        image 'maven:3.8.4-openjdk-11' // Usa la imagen oficial de Maven
                        args '-v /var/run/docker.sock:/var/run/docker.sock' // Monta el socket Docker
                    }
                }
            steps {
                // Construye el proyecto Maven
                sh 'mvn clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Construye la imagen Docker
                    def image = docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                    // Inicia sesión en el registro Docker
                    docker.withRegistry("https://${env.REGISTRY}", 'docker-credentials-id') {
                        // Empuja la imagen al registro
                        image.push()
                    }
                }
            }
        }
        stage('Cleanup') {
            steps {
                // Limpieza después de cada build
                cleanWs()
            }
        }
    }
}
