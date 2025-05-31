pipeline {
    agent any

    stages {
        stage('Setup virtualenv') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Lint') {
            steps {
                sh '''
                    . venv/bin/activate
                    flake8 app/ --count --select=E9,F63,F7,F82 --show-source --statistics
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest tests/
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    . venv/bin/activate
                    python setup.py sdist
                '''
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'dist/*.tar.gz', fingerprint: true
            }
        }
    }

    post {
        failure {
            echo 'Build failed.'
        }
        success {
            echo 'Build succeeded!'
        }
    }
}
