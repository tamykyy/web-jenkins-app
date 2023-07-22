pipeline {
    agent any
    stages {
        stage ('Build') {
            steps {
                withMaven (
                    maven: 'localMaven_3.9.3'
                ) {
                    sh 'mvn clean package'
                }
            }
            post {
                success {
                    archiveArtifacts artifacts:'**/target/*.war'
                }
            }
        }
        stage ('Deploy to staging') {
            steps {
                build 'deploy_to_staging'
            }
        }
        stage ('Deploy to prod') {
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message: 'Approve prod deployment?'
                }
                build 'deploy_to_prod'
            }
        }
    }
}