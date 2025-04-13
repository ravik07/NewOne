pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'Java17'
    }

    environment {
        // This ID must match your Jenkins Credentials ID (Secret Text)
        SONARQUBE_TOKEN = credentials('SonarScanner')
    }

    stages {
        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('BankSonar') {
                    bat 'mvn sonar:sonar -Dsonar.login=%SONARQUBE_TOKEN%'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
