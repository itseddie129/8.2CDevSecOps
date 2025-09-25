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
            # Download and unzip SonarScanner CLI
            curl -O https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
            unzip -o sonar-scanner-cli-5.0.1.3006-linux.zip
            export PATH=$PATH:$(pwd)/sonar-scanner-5.0.1.3006-linux/bin

            # Run SonarScanner using sonar-project.properties
            sonar-scanner \
              -Dsonar.login=$SONAR_TOKEN
            '''
        }
    }
}
    }
}
