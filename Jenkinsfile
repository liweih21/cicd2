pipeline {
    agent any

    environment {
        WEBEX_BOT_TOKEN = credentials('MTczNjY4OGItNWVmMS00MmFjLThkZGQtYTYyM2QzMjdhMmI5Njc2MTVkMzMtMjgx_P0A1_29bfee7a-764c-46a3-810a-8db47e4b026a')
        WEBEX_ROOM_ID   = credentials('Y2lzY29zcGFyazovL3VybjpURUFNOnVzLXdlc3QtMl9yL1JPT00vOGFiMGYzYTAtZjE5Zi0xMWVkLTgzZDQtZWJmYTQwYzg5N2Uw')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/liweih21/cicd2.git'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Run tests') {
            steps {
                sh '. venv/bin/activate && pytest'
            }
        }
    }

    post {
        success {
            sh """
            curl -X POST \
              https://webexapis.com/v1/messages \
              -H "Authorization: Bearer $WEBEX_BOT_TOKEN" \
              -H "Content-Type: application/json" \
              -d '{ "roomId": "$WEBEX_ROOM_ID", "text": "✅ Jenkins build SUCCESS for ${env.JOB_NAME} #${env.BUILD_NUMBER}" }'
            """
        }
        failure {
            sh """
            curl -X POST \
              https://webexapis.com/v1/messages \
              -H "Authorization: Bearer $WEBEX_BOT_TOKEN" \
              -H "Content-Type: application/json" \
              -d '{ "roomId": "$WEBEX_ROOM_ID", "text": "❌ Jenkins build FAILED for ${env.JOB_NAME} #${env.BUILD_NUMBER}" }'
            """
            """
        }
    }
}
