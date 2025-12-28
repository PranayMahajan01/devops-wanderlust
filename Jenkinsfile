pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarCloud Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')
            }
            steps {
                timeout(time: 20, unit: 'MINUTES') {
                    sh '''
                        sonar-scanner \
                        -Dsonar.login=$SONAR_TOKEN \
                        -Dsonar.qualitygate.wait=false
                    '''
                }
            }
        }

        stage('OWASP Dependency Check') {
            steps {
                sh '''
                    dependency-check.sh \
                    --scan . \
                    --format HTML \
                    --out dependency-check-report \
                    --data /tmp/dc-data \
                    --noupdate \
                    --failOnCVSS 11 \
                    --disableAssembly \
                    --disableNodeJS \
                    --disableYarnAudit			
                '''
            }
        }

        stage('Trivy FS Scan') {
            steps {
                sh '''
                    trivy fs . \
                    --severity HIGH,CRITICAL \
                    --format table
                '''
            }
        }

    }
}

