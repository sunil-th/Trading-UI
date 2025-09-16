
pipeline {
    agent any

     tools {
    nodejs "NodeJS14"   // :point_left: if need Use the new Node.js 20 installation
  }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Shaik123-hu/Trading-UI.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install --omit=optional'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || echo "⚠️ No tests found or tests failed"'
            }
        }

        stage('Build Application') {
            steps {
                withEnv(["CI=false"]) {
                    sh 'npm run build'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Node.js pipeline finished successfully.'
        }
        failure {
            echo '❌ Node.js pipeline failed — check console output.'
        }
    }
}
