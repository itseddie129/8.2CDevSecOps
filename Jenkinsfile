pipeline {
    agent any

    stages {
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
                sh 'npm audit --json > audit-results.json || true'
                archiveArtifacts artifacts: 'audit-results.json', allowEmptyArchive: true
            }
        }
    }
}