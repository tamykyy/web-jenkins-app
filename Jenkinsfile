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
                    echo 'Archiving...'
                    archiveArtifacts artifacts:'**/target/*.war'
                }
            }
        }
        stage ('Deploy') {
            steps {
                echo "Deploy step..."
            }
        }
    }
}