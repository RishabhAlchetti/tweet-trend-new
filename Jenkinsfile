pipeline {
    agent {
        node {
            label  'Maven'
        }
    }

 environment{
    PATH = "/opt/apache-maven-3.9.11/bin:$PATH"
 }
    stages {
        stage('build') {
            steps {
               sh 'mvn clean deploy'
            }
        }
        stage('SonarQube analysis') {
        environment {
          scannerHome = tool 'sonar-scanner'
        }
        steps{
    withSonarQubeEnv('Sonar-Server') { // If you have configured more than one global server connection, you can specify its name
      sh "${scannerHome}/bin/sonar-scanner"
    }
    }
  }
}
    }


