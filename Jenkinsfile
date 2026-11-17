@'
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "<yash2303117>/my-web-app"
        CONTAINER_NAME = "my-web-app-container"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yashk514/docker-jenkins-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                    bat 'docker push %DOCKER_IMAGE%'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                bat '''
                    docker stop %CONTAINER_NAME% 2>nul || exit /b 0
                    docker rm %CONTAINER_NAME% 2>nul || exit /b 0
                    docker run -d -p 8081:80 --name %CONTAINER_NAME% %DOCKER_IMAGE%
                '''
            }
        }
    }
}
'@ | Set-Content -Path Jenkinsfile -Encoding UTF8