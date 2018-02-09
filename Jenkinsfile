pipeline {
    agent any

    tools {
        maven 'localMaven'
        jdk 'localJDK'
    }
    
    parameters {
        string(name: 'tomcat_dev', defaultValue: '34.201.170.99', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '52.206.125.247', description: 'Staging Server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage ('Build') {
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    
        stage ('Deployments') {
            parallel{
             stage ('Deploy to Staging'){
                    steps {
                        bat "C:\\\"Program Files (x86)\"\\Linux\\pscp -batch -i C:\\Users\\sidne\\Development\\ssh-key\\tomcat-demo.pem \"C:\\Program Files (x86)\\Jenkins\\workspace\\FullyAutomated@2\\webapp\\target\\webapp.war\" ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ('Deploy to Production') {
                    steps {
                        bat "C:\\\"Program Files (x86)\"\\Linux\\pscp -batch -i C:\\Users\\sidne\\Development\\ssh-key\\tomcat-demo.pem \"C:\\Program Files (x86)\\Jenkins\\workspace\\FullyAutomated@2\\webapp\\target\\webapp.war\" ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}