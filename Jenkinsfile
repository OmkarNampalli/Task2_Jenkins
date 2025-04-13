pipeline {
    agent {
        docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        IMAGE_NAME = 'your-dockerhub-username/myapp'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'docker run --rm $IMAGE_NAME:$IMAGE_TAG npm test'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "$DOCKER_CREDENTIALS_ID", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest
                        docker push $IMAGE_NAME:$IMAGE_TAG
                        docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker stop myapp || true
                    docker rm myapp || true
                    docker run -d --name myapp -p 80:3000 $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        success {
            echo "🚀 Deployment succeeded!"
        }
        failure {
            echo "❌ Deployment failed. Check logs."
        }
    }
}
