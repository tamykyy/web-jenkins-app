pipeline {
    agent any
    stages {
        stage ('Build') {
            steps {
                sh 'export MAVEN_HOME=/opt/homebrew/Cellar/maven/3.9.3/libexec'
                sh 'export PATH=$PATH:$MAVEN_HOME/bin'
                sh 'mvn --version'
                sh 'mvn clean package'
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