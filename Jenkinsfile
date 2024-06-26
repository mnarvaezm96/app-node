pipeline {
    agent any

    environment {
 
        DOCKER_IMAGE = 'mnarvaezm96/app-node'
        GIT_REPO = 'https://github.com/mnarvaezm96/app-node'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone github repo
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependecies
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Exec test
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            when {
                expression {
                    // if test success
                    currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                // Build docker image
                script {
                    docker.build("${env.DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            when {
                expression {
                    // if test success
                    currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                // Upload docker image
                script {
                    docker.withRegistry('https://dockerhub-url', "${DOCKERHUB_CREDENTIALS}") {
                        docker.image("${env.DOCKER_IMAGE}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
    }
}