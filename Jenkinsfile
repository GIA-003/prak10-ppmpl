pipeline {
    agent any
    environment {
        CI = 'true'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Ambil branch sesuai dengan pipeline trigger
                    def selectedBranch = env.BRANCH_NAME ?: 'main'
                    git branch: selectedBranch, url: 'https://github.com/GIA-003/prak10-ppmpl.git'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Unit Tests') {
            when {
                anyOf {
                    branch 'main'
                    branch 'coba1'
                }
            }
            steps {
                bat 'npm test'
            }
        }
        stage('Run Integration Tests') {
            when {
                branch 'main'
            }
            steps {
                bat 'npm run test:integration'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                // Tambahkan perintah build jika diperlukan
            }
        }
        stage('Deploy to Staging') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying to staging server...'
                bat '''
                scp -r ./build user@staging-server:/var/www/app
                ssh user@staging-server "systemctl restart app-service"
                '''
            }
        }
    }
    post {
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
