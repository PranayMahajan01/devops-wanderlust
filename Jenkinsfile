pipeline {pipeline {
    agent any

    options {
        disableResume()
        durabilityHint('PERFORMANCE_OPTIMIZED')
    }

    stages {

        stage('Code') {
            steps {
                checkout scm
            }
        }

        stage('SonarCloud Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')
                NODE_OPTIONS = "--max-old-space-size=512"
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
                  --out dependency-check-report
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
