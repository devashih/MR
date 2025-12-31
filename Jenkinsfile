pipeline {
    agent any

    stages {
        stage('Pull Latest Code') {
            steps {
                sh '''
                cd /opt/MR
                git pull origin main
                '''
            }
        }

        stage('Build & Deploy') {
            steps {
                sh '''
                cd /opt/MR
                docker-compose down
                docker-compose up -d --build
                '''
            }
        }
    }
}

