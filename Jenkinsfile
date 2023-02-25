#!/usr/bin/env groovy

@Library("global-pipeline-libraries@main") _
mavenBuild{
               service="pipeline-library"
               branch="main"
               url="https://github.com/yatheesh-sai-kalimili/jgsu-spring-petclinic.git"
                }
pipeline {
    agent any
    environment {
        RELEASE='20.04'
    }
    triggers { pollSCM('* * * * *') }
    stages {
        // implicit checkout stage
        stage('Audit tools') {
           steps{
               auditTools()
           }
        }
        stage('Build') {
            environment {
                LOG_LEVEL='INFO'
            }
            input {
               message 'Deploy?'
               ok 'Do it!'
               parameters {
                   string(name: 'TARGET_ENVIRONMENT', defaultValue: 'PROD', description: 'Target deployment environment')
               }
            }
            steps {
                echo "Build release ${RELEASE} with log level ${LOG_LEVEL}..."
            }
        }
    }
    // post after stages, for entire pipeline, is also an implicit step albeit with explicit config here, unlike implicit checkout stage
    //post {
    //    always {
    //        junit '**/target/surefire-reports/TEST-*.xml'
    //        archiveArtifacts 'target/*.jar'
    //    }
    //}
}

void auditTools(){
    sh '''
       git version
    '''
}