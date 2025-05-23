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
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'main', credentialsId: 'Jenkins-Github-Token', url: 'https://github.com/avinashvrm03/a-youtube-clone-app.git'
            }
        }
        stage('Sonarqube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube-Server') {
                        sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=a-youtube-clone-app \
                        -Dsonar.projectKey=a-youtube-clone-app"
                    }
                }
                
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialId: 'Jenkins-Sonar-Token'
                }
            }
        }
    }
}
