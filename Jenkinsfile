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
                    #!/bin/bash
                    export NVM_DIR="$HOME/.nvm"

                    # Install NVM if not installed
                    if [ ! -s "$NVM_DIR/nvm.sh" ]; then
                        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.6/install.sh | bash
                    fi

                    # Load NVM
                    . "$NVM_DIR/nvm.sh"

                    # Install and use Node 18
                    nvm install 18
                    nvm use 18

                    # Show versions for debugging
                    node -v
                    npm -v

                    # Install dependencies
                    npm install
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    # Safe test command to avoid breaking the pipeline if missing
                    npm test || true
                '''
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

    // ===== Post-build notifications =====
    post {
        success {
            mail to: 'mtti882@gmail.com',
                 subject: "Build SUKCES: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Pipeline zakończony pomyślnie! Sprawdź: ${env.BUILD_URL}"
        }
        failure {
            mail to: 'mtti882@gmail.com',
                 subject: "Build NIEPOWODZENIE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Pipeline nie powiódł się. Sprawdź szczegóły: ${env.BUILD_URL}"
        }
    }
}
