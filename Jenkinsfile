pipeline {
  agent {
    node {
      label 'maven'
    }
  }
  environment {
    PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
  }

  stages {
    stage('maven-build') {
      steps {
        sh 'mvn clean deploy'
      }
    }
    stage('SonarQube analysis') {
      steps {
        script {
          def workspaceDir = "/home/ubuntu/jenkins/workspace/NewMultibranch_main"
          dir(workspaceDir) {
            withSonarQubeEnv('shak-sonarqube-server') {
              sh "${scannerHome}/bin/sonar-scanner"
            }
          }
        }
      }
    }
  }
}