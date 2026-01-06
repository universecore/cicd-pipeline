stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker image') {
            steps {
                // Using standard shell commands instead of the docker global variable
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Run Tests') {
            steps {
                // Instead of .inside{}, we use 'docker run'
                // --rm removes the container after it exits
                // -v mounts the current workspace so the container can see your code
                sh """
                    docker run --rm -v ${WORKSPACE}:/app -w /app ${IMAGE_NAME}:latest sh -c '
                        echo "Running tests..."
                        if [ -f package.json ]; then
                            npm install && npm test
                        elif [ -f requirements.txt ]; then
                            pip install -r requirements.txt && pytest
                        else
                            echo "No tests found"
                            exit 1
                        fi
                    '
                """
            }
        }
}
