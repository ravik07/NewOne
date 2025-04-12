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

        stage("Quality Gate") {
        	environment {
        		SONAR_HOST_URL = 'http://localhost:8080/'
        		SONAR_AUTH_TOKEN = credentials('SonarScanner')
        	}
            steps {
                bat 'mvn sonar:sonar -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_AUTH_TOKEN'
            }
        }
    }
}
