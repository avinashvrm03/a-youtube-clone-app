pipeline {
    agent { label "Jenkins-Agent" }
    tools {
        jdk 'java21'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tools 'SonarQube-Scanner'
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
                git branch: 'main', credentialsId: 'github' url: 'https://github.com/avinashvrm03/a-youtube-clone-app.git'
                }
            }
        }
    }
}
    post {
        sucessful {
            sh 'echo Pipeline Sucessfully Run'
        }
        Failed {
             sh 'echo Pipeline Failed'
        }
    }
