pipeline {
    agent any

    environment {
        APP_PORT = "5055"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                sh '''
                python3 -m venv venv
                source venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

stage('Run App') {
    steps {
        sh '''
        pkill -f app.py || true
        sleep 2

        nohup venv/bin/python app.py > app.log 2>&1 &

        sleep 5
        lsof -i :5055 || true
        '''
    }
 }
    }

    post {
        success {
            echo "App running at http://127.0.0.1:5055"
        }
        failure {
            echo "Build failed"
        }
    }
}
