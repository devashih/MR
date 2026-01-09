pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "asura0009"
        IMAGE_NAME = "mr-app"
        IMAGE_TAG = "v1"
        DOCKERHUB_CREDENTIALS = "dockerhub-creds"
        CONTAINER_NAME = "mr-container"
    }

    stages {

        stage("Clone GitHub Repo") {
            steps {
                git "https://github.com/devashih/MR.git"
            }
        }

        stage("Build Docker Image") {
            steps {
                sh """
                docker build -t $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG .
                """
            }
        }

        stage("Login to DockerHub") {
            steps {
                withCredentials([usernamePassword(credentialsId: "$DOCKERHUB_CREDENTIALS", usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                }
            }
        }

        stage("Push Image to DockerHub") {
            steps {
                sh """
                docker push $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG
                """
            }
        }

        stage("Stop Old Container") {
            steps {
                sh """
                docker rm -f $CONTAINER_NAME || true
                """
            }
        }

        stage("Pull Latest Image") {
            steps {
                sh """
                docker pull $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG
                """
            }
        }

        stage("Run Container") {
            steps {
                sh """
                docker run -d \
                --name $CONTAINER_NAME \
                -p 80:8000 \
                $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG
                """
            }
        }
    }

    post {
        success {
            echo "🚀 Application deployed successfully!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}
