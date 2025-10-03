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
    }
}

