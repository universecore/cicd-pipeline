pipeline {
    agent any

    environment {
        IMAGE_NAME = "cicd-pipeline-app"
        CI = 'true' 
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Running build script...'
                sh 'chmod +x scripts/build.sh'
                sh './scripts/build.sh'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running test script...'
                sh 'chmod +x scripts/test.sh'
                sh './scripts/test.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs to see if npm or docker is missing.'
        }
        always {
            cleanWs()
        }
    }
}
