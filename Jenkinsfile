pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install --no-progress'
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
                        curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
                        apt-get update && apt-get install -y nodejs

                        node -v
                        npm -v

                        npm install --no-progress sonar-scanner

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

        stage('Snyk Security Test') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh '''
                        npm install -g snyk
                        snyk auth $SNYK_TOKEN
                        snyk test --all-projects --org=itseddie129 || true
                    '''
                }
            }
        }
    }
}
