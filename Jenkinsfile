pipeline {
    agent any

    tools {
        maven 'Maven' 
        jdk 'Java17'  
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

        
    }
}