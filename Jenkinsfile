pipeline {
    agent any

    environment {
        APP_NAME = "smart-parking-app"
        DEV_CONTAINER = "smart-parking-dev"
        DEV_URL = "http://13.58.211.204"
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
<<<<<<< HEAD
=======

    stage('Run testRigor Tests (DEV)') {
      steps {
        withCredentials([string(credentialsId: 'testRigorToken', variable: 'TESTRIGOR_TOKEN')]) {
          sh '''
            echo "Triggering testRigor tests..."

            curl -X POST \
              -H "Content-Type: application/json" \
              -H "auth-token: $TESTRIGOR_TOKEN" \
              --data '{"forceCancelPreviousTesting":true}' \
              https://api.testrigor.com/api/v1/apps/DYnF8LHyz83AeE7vv/retest

            echo "testRigor test triggered successfully"
          '''
        }
      }
    }
  }
>>>>>>> e8de157 (Session/Auth polish + Jenkins updates)

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
