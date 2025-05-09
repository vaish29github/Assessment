pipeline {
    agent any

    environment {
        DOCKER_PATH = 'C:\\Program Files\\Docker\\Docker\\resources\\bin'
        TERRAFORM_PATH = 'C:\\Users\\user\\OneDrive\\Desktop\\terraform_1.11.3_windows_386'
        GIT_PATH = 'C:\\Program Files\\Git\\cmd'
        CMD_PATH = 'C:\\Windows\\System32'
        PATH="${DOCKER_PATH};${TERRAFORM_PATH};${GIT_PATH};${CMD_PATH}${PATH}"

        DOCKERHUB_USERNAME = 'vaishnavibhardwaj'
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub-credentials-id291201'   // Jenkins credentials ID
        IMAGE_NAME = 'assessment2912'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vaish29github/Assessment.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKERHUB_USERNAME%/%IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                bat "docker push %DOCKERHUB_USERNAME%/%IMAGE_NAME%:%IMAGE_TAG%"
            }
        }
    }

    post {
        success {
            echo 'Successfull!'
        }
        failure {
            echo 'Failed'
        }
    }
}
