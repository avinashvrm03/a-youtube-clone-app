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
        DOCKER_PASS = "dockerhub"
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
        stage ('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub') {
                    docker_image = docker.build"${IMAGE_NAME}"
                    }
                    docker.withRegistry('', 'dockerhub') {
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('TRIVY Image Scan') {
            steps {
                script {
                    sh 'docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image avinash0001/a-youtube-clone-app:latest'
                }
            }
        }
        stage('Deploy to Kubernets') {
            steps {
                dir ('Kubernetes') {
                    withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'kubernetes', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                                            sh 'kubectl delete --all pod'
                                            sh 'kubectl apply -f deployment.yml'
                                            sh 'kubectl apply -f service.yml'
                }

              }
            }
        }
    }
    post {
        success {
            echo 'Pipeline Sucessfully Run'
        }
        failure {
             echo 'Pipeline Failed'
        }
    }
}
