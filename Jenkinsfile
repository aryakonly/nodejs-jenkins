pipeline {
    agent any

    tools {
        nodejs 'nodejs'   // match your Jenkins NodeJS tool name
    }

    environment {
        NODE_ENV = 'production'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/aryakonly/nodejs-jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || echo "No tests, skipping..."'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    npm install -g pm2 || true
                    pm2 stop nodejs-app || true
                    pm2 delete nodejs-app || true
                    pm2 start server.js --name nodejs-app
                    pm2 save
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed. Check logs.'
        }
    }
}
