pipeline {
    agent any

    environment {
        APP_NAME = "smart-parking-app"
        DEV_CONTAINER = "smart-parking-dev"
        DEV_URL = "http://13.58.211.204"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out code..."
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building application..."
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying to DEV..."
            }
        }
    }

    post {
        success {
            withCredentials([string(credentialsId: 'SLACK_WEBHOOK', variable: 'SLACK_WEBHOOK')]) {
                sh """
                curl -X POST -H 'Content-type: application/json' \
                --data '{\"text\":\"✅ Jenkins Build #${env.BUILD_NUMBER}\\nSmart Parking deployed successfully\\nDEV URL: http://13.58.211.204\"}' \
                "$SLACK_WEBHOOK"
                """
            }
        }

        failure {
            withCredentials([string(credentialsId: 'SLACK_WEBHOOK', variable: 'SLACK_WEBHOOK')]) {
                sh '''
                curl -X POST -H 'Content-type: application/json' \
                --data "{\"text\":\"❌ Jenkins Build #$BUILD_NUMBER FAILED\nSmart Parking pipeline encountered an error.\"}" \
                "$SLACK_WEBHOOK"
                '''
            }
        }

        always {
            echo "========================================"
            echo "Application is live at: http://13.58.211.204"
            echo "========================================"
        }
    }
}
