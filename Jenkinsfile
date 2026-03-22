pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Pobieranie aplikacji Node.js z GitHub...'
                git branch: 'main',
                    url: 'https://github.com/heroku/node-js.git'
                    credentialsId: ''
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
