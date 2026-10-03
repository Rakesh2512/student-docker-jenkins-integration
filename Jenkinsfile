pipeline {

    agent any

    environment {
        APP_NAME = 'student'
        APP_PORT = '8082'
        CONTAINER_NAME = 'student-container'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Spring Boot application...'
                bat 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                bat 'docker build -t %APP_NAME%:%BUILD_NUMBER% .'

                echo "Docker image created: %APP_NAME%:%BUILD_NUMBER%"
            }
        }

        stage('Stop Old Container') {
            steps {
                echo 'Stopping old container if it exists...'

                bat 'docker rm -f %CONTAINER_NAME% 2>nul || exit /b 0'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting new Docker container...'

                bat 'docker run -d --name %CONTAINER_NAME% -p %APP_PORT%:%APP_PORT% %APP_NAME%:%BUILD_NUMBER%'
            }
        }

        stage('Verify') {
            steps {
                echo 'Checking running containers...'

                bat 'docker ps'

                echo 'Checking application...'

                //bat 'curl http://localhost:%APP_PORT%'
            }
        }
    }

    post {

        success {
            echo '======================================'
            echo 'PIPELINE SUCCESSFUL!'
            echo '======================================'
            echo "Application: %APP_NAME%"
            echo "Docker Image: %APP_NAME%:%BUILD_NUMBER%"
            echo "Port: %APP_PORT%"
        }

        failure {
            echo '======================================'
            echo 'PIPELINE FAILED!'
            echo '======================================'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}