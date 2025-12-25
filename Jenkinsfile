pipeline {
    agent any

    options {
        disableResume()
    }

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
                timeout(time: 10, unit: 'MINUTES') {
                    sh '''
                        sonar-scanner \
                        -Dsonar.login=$SONAR_TOKEN \
                        -Dsonar.qualitygate.wait=false
                    '''
                }
            }
        }

    }
}

