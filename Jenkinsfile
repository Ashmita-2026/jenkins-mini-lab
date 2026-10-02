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

        stage('Test & Coverage') {
    steps {
        sh '.jenkins-venv/bin/pytest --cov=. --cov-report=term --cov-report=xml'
    }
}
stage('Publish Coverage') {
    steps {
        recordCoverage(
            tools: [[parser: 'COBERTURA', pattern: 'coverage.xml']]
        )
    }
}
stage('Lint') {
    steps {
        sh '.jenkins-venv/bin/ruff check app.py test_app.py'
    }
}

    }

    post {
        always {
            sh 'rm -rf .jenkins-venv'
        }
    }
}