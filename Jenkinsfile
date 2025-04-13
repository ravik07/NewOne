pipeline {
    agent any

    tools {
        maven 'Maven' 
        jdk 'Java17'  
    }

    environment {
        // Global SonarQube env vars (optional)
        // Set in Jenkins credentials as Secret Text
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
                    bat 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // This step waits for the Quality Gate result
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
