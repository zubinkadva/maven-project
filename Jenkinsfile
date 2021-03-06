#!/usr/bin/env groovy

pipeline {
    agent any

    parameters {
        string(name: 'tomcat_dev', defaultValue: '13.59.206.203', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '18.216.239.84', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *')
     }

    stages {
        stage('Build') {
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

        stage('Deployment') {
            parallel {
                stage ('Deploy to Staging') {
                    steps {
                        bat "echo y | pscp -scp -i C:/Users/zubin.kadva/Downloads/UdemyJenkins/tomcat-demo-private.ppk \"C:/Program Files (x86)/Jenkins/workspace/fully-automated/webapp/target/*.war\" ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production") {
                    steps {
                        bat "echo y | pscp -scp -i C:/Users/zubin.kadva/Downloads/UdemyJenkins/tomcat-demo-private.ppk \"C:/Program Files (x86)/Jenkins/workspace/fully-automated/webapp/target/*.war\" ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}