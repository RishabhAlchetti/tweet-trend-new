pipeline {
    agent {
        node {
            label  'Maven'
        }
    }

    stages {
        stage('build') {
            steps {
               sh 'mvn clean install'
            }
        }
    }
}

