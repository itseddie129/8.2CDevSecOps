pipeline {
    agent any

    environment {
        NOTIFY_EMAIL = 's223547799@deakin.edu.au' 
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/itseddie129/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'echo "Pretend we are generating a coverage report..."'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit --json > audit-report.log || true'
            }
            post {
                always {
                    emailext(
                        to: "${env.NOTIFY_EMAIL}",
                        subject: "NPM Audit Report: ${currentBuild.currentResult}",
                        body: "Check attached npm audit report.",
                        attachmentsPattern: 'audit-report.log',
                        attachLog: true
                    )
                }
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        npx sonar-scanner \
                          -Dsonar.projectKey=itseddie129_8.2CDevSecOps \
                          -Dsonar.organization=itseddie129 \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=https://sonarcloud.io \
                          -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Snyk Security Scan') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh '''
                        npm install -g snyk
                        snyk auth $SNYK_TOKEN
                        snyk test || true
                    '''
                }
            }
        }
    }

    post {
        success {
            emailext(
                to: "${env.NOTIFY_EMAIL}",
                subject: "BUILD SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build succeeded! Full log is attached.",
                attachLog: true
            )
        }
        failure {
            emailext(
                to: "${env.NOTIFY_EMAIL}",
                subject: "BUILD FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build failed. Check attached log.",
                attachLog: true
            )
        }
    }
}
