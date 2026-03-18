pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Setup Python') {
      steps {
        sh '''
          python3 -V
          python3 -m venv .venv
          . .venv/bin/activate
          python -m pip install --upgrade pip
          pip install -r requirements.txt
        '''
      }
    }

    stage('Run Tests') {
      steps {
        sh '''
          . .venv/bin/activate
          pytest -q
        '''
      }
    }
    stage('Run App') {
    steps {
        sh '''
        . .venv/bin/activate
        nohup python app.py > app.log 2>&1 &
        '''
    }
}
  }

  post {
    always { cleanWs() }
  }
}
