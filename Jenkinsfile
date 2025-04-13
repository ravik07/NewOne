pipeline {
    agent any

    tools {
        maven 'Maven'       // Ensure this matches the name in Jenkins Global Tools
        jdk 'Java17'        // Ensure this matches the name in Jenkins Global Tools
    }

    environment {
        SONAR_TOKEN = credentials('SonarScanner') // Make sure this credential exists in Jenkins
    }

    stages {

        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('BankSonar') { // 'BankSonar' must match your Jenkins Sonar server name
                    bat "mvn sonar:sonar -Dsonar.login=%SONAR_TOKEN%"
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
