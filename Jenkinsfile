pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "vite-hello"
        CONTAINER_NAME = "vite-hello-container"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/saadactin/cicd.git'
            }
        }

        stage('Install Node (if needed)') {
            steps {
                sh '''
                # Install Node.js 20 if not installed
                if ! command -v node >/dev/null 2>&1; then
                  curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
                  sudo apt-get install -y nodejs
                fi
                node -v
                npm -v
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                echo "Building Docker image..."
                docker build -t $DOCKER_IMAGE .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                echo "Stopping old container (if exists)..."
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true

                echo "Running new container..."
                docker run -d --name $CONTAINER_NAME -p 5173:5173 $DOCKER_IMAGE
                '''
            }
        }
    }

    post {
        success {
            echo "🚀 App is running at http://<your-public-ip>:5173"
        }
        failure {
            echo "❌ Build failed."
        }
    }
}
