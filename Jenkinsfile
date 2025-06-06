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
      withSonarQubeEnv("SonarQube-Server") {
        sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=a-youtube-clone-app -Dsonar.projectKey=a-youtube-clone-app"
      }
    }
  }
  }
  stage('Quality Gate') {
    steps {
      waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-token'
    }
  }
  stage('Install Dependencies') {
    steps {
      sh "npm install"
    }
  }
  stage('OWASP FS SCAN') {
    steps {
      dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit', odcInstallation: 'owaspdc'
      dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
    }
  }
  }
  post {
    success {
      sh 'The Pipeline is Sucessfull'
    }
    failure {
        sh 'The Pipeline is Failed'
    }
  }
}

