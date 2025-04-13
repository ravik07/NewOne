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
            	echo '🔍 Running SonarQube analysis...'
                withSonarQubeEnv('BankSonar') {
                    bat 'mvn sonar:sonar -Dsonar.projectKey=banking-app -Dsonar.host.url=http://localhost:9000'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo '✅ Checking Quality Gate...'
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
    
    post {
        success {
            emailext(
                subject: "✅ SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>🎉 Build Successful!</p>
                         <p><b>Project:</b> ${env.JOB_NAME}</p>
                         <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                         <p><b>Build URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                         <p><b>Sonar Dashboard:</b> <a href="http://localhost:9000/dashboard?id=banking-app">View Report</a></p>""",
                mimeType: 'text/html',
                to: 'tecnoflank@gmail.com'
            )
        }
        
        failure {
            emailext(
                subject: "❌ FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>❗ Build Failed</p>
                         <p><b>Project:</b> ${env.JOB_NAME}</p>
                         <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                         <p><b>Build URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html',
                to: 'tecnoflank@gmail.com'
            )
        }
    }
}