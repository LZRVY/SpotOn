pipeline {
  agent any

  options {
    skipDefaultCheckout(true)
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
          venv/bin/python -m pip install --upgrade pip
          venv/bin/python -m pip install --only-binary=:all: Flask==2.3.3 gunicorn==21.2.0 psycopg2-binary==2.9.9
        '''
      }
    }

    stage('Run App') {
      steps {
        sh '''
          export DATABASE_URL="postgresql://postgres:NewStrongPassword123@smart-parking-dev.c6jw6cu8i2cr.us-east-1.rds.amazonaws.com:5432/postgres?sslmode=require"

          pkill -f app.py || true
          nohup venv/bin/python app.py > app.log 2>&1 &
          sleep 5
          lsof -i :5055 || echo "App not running"
        '''
      }
    }

  }
}