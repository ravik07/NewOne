pipeline {
    agent any

    tools {
        maven 'Maven' 
        jdk 'Java17'  
    }

    environment {
        SONARQUBE = 'SonarQube' 
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ravik07/NewOne.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('BankSonar') {
                    sh 'mvn sonar:sonar'
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
