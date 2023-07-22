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
                        copyArtifacts filter: '**/*.war', projectName: 'test-pipeline-as-code'
                        deploy adapters: [tomcat9(credentialsId: '5dfa1126-f113-4cce-824f-31fe1bbcb36e', path: '', url: 'http://localhost:8090')], contextPath: null, onFailure: false, war: '**/*.war'                    }
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