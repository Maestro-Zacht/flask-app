pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('de89d08f-325f-4f17-81e5-a35e721c1a55') // Use the ID of your credentials
    }
    stages {
        stage('Lint') {
            steps {
                sh '''
                #!/bin/bash
                python3 -m venv venv
                . ./venv/bin/activate
                pip install flake8
                flake8 app.py
                '''
            }
        }
        stage('Format') {
            steps {
                sh '''
                #!/bin/bash
                python3 -m venv venv
                . ./venv/bin/activate
                pip install black
                black --check app.py
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin'
                sh 'docker build --network host -t maestrozacht/flask-app:0.0.1 .'
                sh 'docker push maestrozacht/flask-app:0.0.1'
            }
        }
    }
}