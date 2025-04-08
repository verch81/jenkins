pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/verch81/jenkins.git'
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
            emailext (
                to: 'InsertYour@Mail.Here',
                subject: "Jenkins Pipeline: ${currentBuild.fullDisplayName}",
                body: """<p>Build result: ${currentBuild.currentResult}</p>
                         <p>Check console output at <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html'
            )
        }
    }
}
