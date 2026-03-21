pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Pobieranie kodu...'
                git 'https://github.com/heroku/node-js.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test || true'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    mkdir -p deploy
                    cp -r * deploy/
                '''
            }
        }
    }
}                
