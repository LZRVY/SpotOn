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
          . venv/bin/activate
          pip install -r requirements.txt
        '''
      }
    }

    stage('Setup DB') {
      steps {
        sh '''
          dropdb --if-exists smart_parking || true
          createdb smart_parking || true
          psql smart_parking < dump.sql
        '''
      }
    }

    stage('Run Application') {
    steps {
        sh '''
            pkill -f app.py || true
            sleep 2

            nohup venv/bin/python app.py > app.log 2>&1 &
            sleep 5

            lsof -i :5055 || echo "App not running"
        '''
    }
}

  }
}