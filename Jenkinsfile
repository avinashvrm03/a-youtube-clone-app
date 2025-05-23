pipeline {
    agent { label "Jenkins-Agent" }
    tools {
        jdk 'java21'
        nodejs 'node16'
    }
    environment {
    SCANNER_HOME = tool 'Sonarqube-Scanner'
    APP = "a-youtube-clone-app"
    DOCKER_USER = "avinash0001"
    IMAGE_NAME = "${DOCKER_USER}/${APP}"
    IMAGE_TAG = ""
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
                    waitForQualityGate abortPipeline: false, credentialsId: 'Jenkins-Sonar-Token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('', 'Jenkins-Dockerub-Token') {
                        docker_image = docker.build"${IMAGE_NAME}"
                    }
                    docker.withRegistry('','Jenkins-Dockerub-Token') {
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('TRIVY Image Scan') {
            steps {
                script {
                    sh "docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image avinash0001/a-youtube-clone-app:latest"
                }
            }
        }
    }
}
