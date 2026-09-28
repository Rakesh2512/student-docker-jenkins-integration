pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t student:latest .'
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker rm -f student-container 2>nul'
                bat 'docker run -d --name student-container -p 8082:8082 student:latest'
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