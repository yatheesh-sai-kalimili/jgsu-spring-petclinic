#!/usr/bin/env groovy
// shebang tells most editors to treat as groovy (syntax highlights, formatting, etc)

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
                sh './mvnw clean package'
                echo "Build release ${RELEASE} with log level ${LOG_LEVEL}..."
            }
        }
    }
    // post after stages, for entire pipeline, is also an implicit step albeit with explicit config here, unlike implicit checkout stage
    post {
        always {
            junit '**/target/surefire-reports/TEST-*.xml'
            archiveArtifacts 'target/*.jar'
        }
    }
}

void auditTools(){
    sh '''
       git version
       docker version
    '''
}