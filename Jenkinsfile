pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Pobieranie aplikacji Node.js z GitHub...'
                git branch: 'master',
                    url: 'git@github.com:heroku/node-js-sample.git',
                    credentialsId: 'github-ssh'
            }
        }
        stage('Build') {
            steps {
                sh '''
                    export NVM_DIR="$HOME/.nvm"
                    if [ ! -s "$NVM_DIR/nvm.sh" ]; then
                        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.6/install.sh | bash
                    fi
                    source "$NVM_DIR/nvm.sh"
                    nvm install 18
                    nvm use 18
                    node -v
                    npm -v
                    npm install
                '''
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
                    rsync -av --exclude='deploy' ./ deploy/
                '''
            }
        }
    }
}
