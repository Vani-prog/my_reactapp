pipeline {
    agent any

    triggers {
        cron('* * * * *')   // every 5 minutes (better than every minute)
    }

    environment {
        IMAGE_NAME = "vanireddy2025/my_reactapp"
        DOCKERHUB_CRED = credentials('dockerhub')
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Vani-prog/my_reactapp.git',
                    branch: 'main'
            }
        }

        stage('Install Dependencies & Build React') {
            steps {
                sh '''
                  npm install
                  npm run build
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ React Docker image built and pushed successfully"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
