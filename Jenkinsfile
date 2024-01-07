pipeline {
    agent {
        node {
            label 'maven'
        }
    }
 environment {
    PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
 }

    stages{
        stage('maven-build'){
         steps{
             sh 'mvn clean deploy'
         } 
       }

     stages {
        stage('Debug') {
            steps {
                script {
                    def workspacePath = pwd()
                    echo "Current Directory: ${workspacePath}"
                    echo "Contents of Workspace:"
                    sh 'ls -al'
                }
            }
        }

    stage('SonarQube analysis') {
    environment {
        scannerHome = tool 'shak-sonar-scanner' 
    }     
    steps {
        script {
            def workspacePath = pwd()
            withSonarQubeEnv('shak-sonarqube-server') {
                sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectBaseDir=${workspacePath}"
            }
        }
    }
}

}
}


   