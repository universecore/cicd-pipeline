pipeline {
    agent any

    environment {
        IMAGE_NAME = "cicd-pipeline-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:latest").inside {
                        sh '''
                            echo "Running tests..."
                            if [ -f package.json ]; then
                              npm install
                              npm test
                            elif [ -f requirements.txt ]; then
                              pip install -r requirements.txt
                              pytest
                            else
                              echo "No tests found"
                              exit 1
                            fi
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            cleanWs()
        }
    }
}
