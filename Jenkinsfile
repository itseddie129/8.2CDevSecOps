pipeline {
    agent any

    environment {
        NOTIFY_EMAIL = 'eddieeee23@gmail.com'
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
                        subject: "Test Stage Completed: ${currentBuild.currentResult}",
                        body: """Hello,<br>
The <b>Test</b> stage has finished.<br>
Check attached logs for details.<br>
Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a>""",
                        attachmentsPattern: 'test-output.log'
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
                        subject: "npm Audit Stage Completed: ${currentBuild.currentResult}",
                        body: """Hello,<br>
The <b>npm audit</b> stage has finished.<br>
Check attached report for vulnerabilities.<br>
Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a>""",
                        attachmentsPattern: 'audit-report.log'
                    )
                }
            }
        }
    }

    post {
        always {
            script {
                try {
                    emailext(
                        to: "${env.NOTIFY_EMAIL}",
                        subject: "Build Completed: ${currentBuild.currentResult} - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """Hello,<br>
The pipeline build has <b>${currentBuild.currentResult}</b>.<br>
Check Jenkins console for logs:<br>
<a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a>""",
                        attachLog: true
                    )
                } catch (err) {
                    echo "Failed to send build email: ${err}"
                }
            }
        }
    }
}
