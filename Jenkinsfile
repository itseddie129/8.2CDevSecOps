pipeline {
    agent any

    environment {
        NOTIFY_EMAIL = 's223547799@deakin.edu.au'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AyaanSiddiqui564/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building..."'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Running tests..." > test-output.log'
            }
            post {
                always {
                    emailext(
                        to: "${env.NOTIFY_EMAIL}",
                        subject: "Test stage completed: ${currentBuild.currentResult}",
                        body: "Check attached logs from the Test stage.",
                        attachmentsPattern: 'test-output.log',
                        attachLog: true
                    )
                }
            }
        }

        stage('Audit') {
            steps {
                sh 'npm audit --audit-level=moderate > audit-report.log || true'
            }
            post {
                always {
                    emailext(
                        to: "${env.NOTIFY_EMAIL}",
                        subject: "npm Audit Report: ${currentBuild.currentResult}",
                        body: "Check attached npm audit report.",
                        attachmentsPattern: 'audit-report.log',
                        attachLog: true
                    )
                }
            }
        }
    }


    post {
        always {
            script {
                // Use try/catch to prevent Jenkins from skipping the email
                try {
                    emailext(
                        to: "${env.NOTIFY_EMAIL}",
                        subject: "Build Completed: ${currentBuild.currentResult} - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The build has finished. Check console and attached logs if needed.",
                        attachLog: true
                    )
                } catch (err) {
                    echo "Failed to send build email: ${err}"
                }
            }
        }
    }
}
