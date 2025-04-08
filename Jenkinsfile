pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/verch81/jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'python3 -m venv $VENV_DIR'
                sh './$VENV_DIR/bin/pip install --upgrade pip'
                sh "./$VENV_DIR/bin/pip install -r requirements.txt"
            }
        }

        stage('Run Tests') {
            steps {
                sh "./$VENV_DIR/bin/pytest --junitxml=results.xml"
                sh "pytest test_calculator.py --verbose"
            }
        }

        stage('Publish Test Results') {
            steps {
                junit 'results.xml'
            }
        }
    }
   
    post {
        always {
            archiveArtifacts artifacts: 'results.xml', fingerprint: true
            cleanWs()
        }
    }
}