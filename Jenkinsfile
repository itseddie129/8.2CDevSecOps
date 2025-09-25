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

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    npm install sonar-scanner && \
                    npx sonar-scanner \
                    -Dsonar.organization=itseddie129 \
                    -Dsonar.projectKey=itseddie129_8.2CDevSecOps \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
}
