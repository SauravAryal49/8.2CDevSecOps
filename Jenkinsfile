pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SauravAryal49/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    emailext (
                        subject: "Test Stage - ${currentBuild.currentResult}: Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Run Tests stage completed with status: ${currentBuild.currentResult}.\n\nSee attached log for details.",
                        to: "aryalsau7@gmail.com",
                        attachLog: true
                    )
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    emailext (
                        subject: "Security Scan Stage - ${currentBuild.currentResult}: Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The NPM Audit (Security Scan) stage completed with status: ${currentBuild.currentResult}.\n\nSee attached log for details.",
                        to: "aryalsau7@gmail.com",
                        attachLog: true
                    )
                }
            }
        }
    }
}