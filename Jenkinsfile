pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                bat 'C:\Users\sidne\Development\apache-maven-3.5.2\bin\mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}