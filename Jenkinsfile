pipeline {
    agent any
    environment{
        APP_NAME:'student'
        APP_PORT:'8082'
    }

    stages {

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t %APP_NAME%:latest .'
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker rm -f student-container 2>nul'
                bat 'docker run -d --name student-container -p %APP_PORT%:%APP_PORT% %APP_NAME%:latest'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}