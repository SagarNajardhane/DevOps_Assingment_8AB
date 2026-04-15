pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = "SagarNajardhane" 
        IMAGE_NAME = "app"
        DOCKER_HUB_CREDS = 'docker-hub-creds' 
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                script {
                    bat "docker build -t %DOCKER_HUB_USER%/%IMAGE_NAME%:%BUILD_NUMBER% ."
                    bat "docker tag %DOCKER_HUB_USER%/%IMAGE_NAME%:%BUILD_NUMBER% %DOCKER_HUB_USER%/%IMAGE_NAME%:latest"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: "${DOCKER_HUB_CREDS}", 
                        passwordVariable: 'DOCKER_PASS', 
                        usernameVariable: 'DOCKER_USER'
                    )]) {

                        bat """
                        echo %DOCKER_PASS%> pass.txt
                        docker login -u %DOCKER_USER% --password-stdin < pass.txt
                        docker push %DOCKER_HUB_USER%/%IMAGE_NAME%:%BUILD_NUMBER%
                        docker push %DOCKER_HUB_USER%/%IMAGE_NAME%:latest
                        docker logout
                        del pass.txt
                        """
                    }
                }
            }
        }
    }
}
