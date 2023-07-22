pipeline {
    agent any
    triggers {
        pollSCM('*/5 * * * *')
    }
    stages {
        stage('Build') {
            steps {
                withMaven(
                        maven: 'localMaven_3.9.3'
                ) {
                    sh 'mvn clean package'
                }
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        copyArtifacts projectName: 'test-pipeline-as-code', selector: lastSuccessful()
                        deploy contextPath: '**/target/*.war', onFailure: false, war: '**/*.war'
                    }
                }
                stage("Deploy to Production") {
                    steps {
                        timeout(time: 5, unit: 'DAYS') {
                            input message: 'Approve prod deployment?'
                        }
                        build 'deploy_to_prod'
                    }
                }
            }
        }
    }
}