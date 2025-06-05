pipeline {
agent any 
tools {
jdk 'java21'
nodejs 'node16'
}
environment {
SCANNER_HOME = tool "SonarQube-Scanner"
}
stages {
  stage('Cleanup Workspace') {
  steps {
    cleanWs()
  }
}
  stage('Checkout SCM ') {
    steps {
      git branch: 'main', credentialsId: 'github', url: 'https://github.com/avinashvrm03/a-youtube-clone-app.git'
    }
  }
  stage('SonarQube Analysis') {
  steps {
    script {
      withSonarEnv("SonarQube-Server") {
        sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=a-youtube-clone-app -Dsonar.projectKey=a-youtube-clone-app"
      }
    }
  }
  }
  }
}

