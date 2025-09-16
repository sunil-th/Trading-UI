pipeline {
    agent any
    tools {
        nodejs 'NodeJS'   // exactly what is configured in Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                echo ":inbox_tray: Checking out code"
                git branch: 'master', url: 'https://github.com/sunil-th/Trading-UI.git'
            }
        }
        stage('Install & Build') {
            steps {
                echo ":package: Installing dependencies and building project"
                sh """
                    npm install
                    npm audit fix || true
                    CI=false npm run build
                """
            }
        }
        stage('Deploy with PM2') {
            steps {
                echo ":rocket: Deploying with PM2"
                sh """
                    # Install pm2 and serve globally, if not already present
                    npm install -g pm2 serve
                    if [ -d "build" ]; then
                        pm2 delete Trading-UI || true
                        pm2 start serve --name Trading-UI -- -s build -l tcp://0.0.0.0:3000
                    else
                        echo ":x: Build folder does not exist. Deployment skipped."
                        exit 1
                    fi
                """
            }
        }
    }
    post {
        success {
            echo ":white_check_mark: Build and Deploy succeeded"
            // you can add slackSend here if needed
        }
        failure {
            echo ":x: Pipeline failed"
            // slackSend or other notification here
        }
    }
}
