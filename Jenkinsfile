pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-html-app"
        CONTAINER_NAME = "my-html-container"
        PORT = "9090"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                sh '''
                if [ $(docker ps -aq -f name=$CONTAINER_NAME) ]; then
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                fi
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d \
                -p $PORT:80 \
                --name $CONTAINER_NAME \
                --restart always \
                $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment Successful 🚀"
            echo "Application running at: http://localhost:${PORT}"
        }
        failure {
            echo "Deployment Failed ❌"
        }
    }
}

