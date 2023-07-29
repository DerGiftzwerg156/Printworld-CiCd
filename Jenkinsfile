pipeline {
    agent any
    tools {
        maven 'Maven'
    }

    stages {
        stage('Clone Spring Boot Repo') {
            steps {
                git branch: 'main', credentialsId: '73f225e1-d321-4d2b-bcf0-2bdcdb4b5128', url: 'https://github.com/DerGiftzwerg156/PrintWorldShop_New.git'
            }
        }
        stage('Build Spring Boot') {
            steps {
                sh '''
                    cd packages/shop-backend
                    mvn clean package
                '''
            }
        }
        stage('Build Spring Boot Docker Image') {
            steps {
                script {
                    def dockerImageName = "your-docker-registry/your-spring-boot-image:${env.BUILD_NUMBER}"
                    docker.build dockerImageName, '-f packages/shop-backend/Dockerfile .'
                }
            }
        }
        stage('Push Spring Boot Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://your-docker-registry', 'docker-credentials-id') {
                        dockerImageName.push()
                    }
                }
            }
        }
    }
}
