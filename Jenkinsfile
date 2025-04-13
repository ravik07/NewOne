pipeline {
    agent any

    tools {
        maven 'Maven' 
        jdk 'Java17'  
    }

    environment {
        SONARQUBE = 'BankSonar' 
    }

    stages {

        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SONARQUBE') {
                    bat 'mvn sonar:sonar'
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}