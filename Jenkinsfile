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
                sh '''
                    sonar-scanner \
                    -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }

    }
}
