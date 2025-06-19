pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/karuna299/my-python-app.git'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh '''
                    python3 -m venv venv
                    ./venv/bin/pip install --upgrade pip --break-system-packages
                    ./venv/bin/pip install -r requirements.txt --break-system-packages
                    ./venv/bin/python -m pytest tests/test_app.py
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-cd-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker rm -f flask-cd || true
                    docker run -d --name flask-cd -p 5000:5000 flask-cd-app
                '''
            }
        }
    }
}
