pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKER_IMAGE = "fidonoso/my-spring-boot-app"
        REGISTRY = "docker.io" // e.g., Docker Hub or any other registry
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
                    image 'maven:3.8.4-openjdk-11' 
                    args '-v /var/run/docker.sock:/var/run/docker.sock' 
                }
            }
            environment {
                MAVEN_HOME = '/usr/share/maven'
            }
            steps {
                // dir('/var/jenkins_home/workspace/test1@2'){
                    sh 'mvn clean package'
                // }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo pwd
                    def imageName = "${DOCKER_IMAGE}:${env.BUILD_ID}"
                    def image = docker.build(imageName)
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-credentials-id') {
                        image.push()
                    }
                }
            }
        }
        stage('Cleanup') {
            steps {
                cleanWs()
            }
        }
    }
}
