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
    CleanWS()
  }
}
  stage('Checkout SCM ') {
    steps {
      git branch: 'main', credentialsId: 'github', url: 'https://github.com/avinashvrm03/a-youtube-clone-app.git'
    }
  }
  stage('SonarQube Analysis') {
  steps {
    sh "echo this is sonarqube analysis stage"
  }
  }
  }
}

