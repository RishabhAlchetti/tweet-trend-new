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
        

    stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'shak-sonar-scanner' 
            }     
            steps {
                script {
                    echo "Before entering withSonarQubeEnv block:"
                    sh 'pwd'
                    
                    def workspacePath = pwd()
                    echo "Current Directory: ${workspacePath}"
                    echo "Contents of Workspace:"
                    sh 'ls -al'

                    withSonarQubeEnv('shak-sonarqube-server') {
                        echo "Inside withSonarQubeEnv block:"
                        sh 'pwd'
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectBaseDir=${workspacePath}"
                    }

                    echo "After withSonarQubeEnv block:"
                    sh 'pwd'
        }
    }
        }
    }
}




   