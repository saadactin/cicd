pipeline {
    agent any

    environment {
        IMAGE_NAME = "vite-hello"
        CONTAINER_NAME = "vite-hello-container"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/saadactin/cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Run Docker Container') {
            steps {
                // Stop & remove if already running
                sh '''
                  docker stop ${CONTAINER_NAME} || true
                  docker rm ${CONTAINER_NAME} || true
                  docker run -d -p 5173:5173 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                '''
            }
        }
    }

    post {
        success {
            echo "✅ App deployed! Visit http://<your-server-public-ip>:5173"
        }
        failure {
            echo "❌ Build failed."
        }
    }
}
