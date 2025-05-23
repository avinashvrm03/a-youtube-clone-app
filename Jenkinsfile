pipeline {
    agent { label "Jenkins-Agent" }
    tools {
        jdk 'java21'
        nodejs 'node16'
    }
    environment {
    SCANNER_HOME = tool 'Sonarqube-Scanner'
    }
    stages {
        stage('Cleanup Workspace') {
            steps {
                cleamupWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'main', credentialsId: 'Jenkins-Github-Token', url: 'https://github.com/avinashvrm03/a-youtube-clone-app.git'
            }
        }
        stage('') {
            steps {
                
            }
        }
    }
}
