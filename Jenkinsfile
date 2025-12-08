pipeline {
    agent any

    environment {
        WEBEX_BOT_TOKEN = credentials('WEBEX_BOT_TOKEN')
        WEBEX_ROOM_ID = 'Y2lzY29zcGFyazovL3VybjpURUFNOnVzLXdlc3QtMl9yL1JPT00vOGFiMGYzYTAtZjE5Zi0xMWVkLTgzZDQtZWJmYTQwYzg5N2Uw'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/liweih21/cicd2.git'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Run tests') {
            steps {
                sh 'pytest -v'
            }
        }
    }

    post {
        success {
            sh """
            curl -X POST https://webexapis.com/v1/messages \
              -H "Authorization: ${WEBEX_BOT_TOKEN}" \
              -H "Content-Type: application/json" \
              -d '{"roomId": "${WEBEX_ROOM_ID}", "text": "Build SUCCESS – Tests passed!"}'
            """
        }

        failure {
            sh """
            curl -X POST https://webexapis.com/v1/messages \
              -H "Authorization: ${WEBEX_BOT_TOKEN}" \
              -H "Content-Type: application/json" \
              -d '{"roomId": "${WEBEX_ROOM_ID}", "text": "Build FAILED – Check Jenkins logs"}'
            """
        }
    }
}
