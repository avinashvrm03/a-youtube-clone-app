pipeline {
    agent { label "Jenkins-Agent" }
    tools {
        jdk 'java21'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tool 'SonarQube-Scanner'
        APP = "a-youtube-clone-app"
        DOCKER_USER = "avinash0001"
        IMAGE_NAME = "${DOCKER_USER}/${IMAGE_NAME}"
    }
    stages {
        stage ('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage ('Checkout from Git') {
            steps {
                script {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/avinashvrm03/a-youtube-clone-app.git'
                }
            }
        }
        stage ('Sonarqube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube-Server') {
                       sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=a-youtube-clone-app -Dsonar.projectKey=a-youtube-clone-app"
                    }
                }
                
            }
        }
        stage ('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: false, credentialsId: "Jenkins-SonarQube-Token"
            }
        }
        stage ('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }
    }
    post {
        success {
            sh 'echo Pipeline Sucessfully Run'
        }
        failure {
             sh 'echo Pipeline Failed'
        }
    }
}
