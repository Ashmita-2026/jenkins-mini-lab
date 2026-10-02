pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv .jenkins-venv
                    .jenkins-venv/bin/pip install --upgrade pip
                    .jenkins-venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '.jenkins-venv/bin/pytest'
            }
        }

    }

    post {
        always {
            sh 'rm -rf .jenkins-venv'
        }
    }
}