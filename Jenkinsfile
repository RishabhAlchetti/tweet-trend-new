pipeline {
    agent {
        node {
            label 'maven'
        }
    }

 //   options {
  //      skipDefaultCheckout(true)
   // }

    environment {
        PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
    }

    stages {
        stage('maven-build') {
            steps {
                script {
                    sh 'mvn clean deploy'
                }
            }
        }

     /*   stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'shak-sonar-scanner'
            }
            steps {
                script {
                    sh 'mvn verify sonar:sonar -Dsonar.organization=shak-key -Dsonar.projectKey=shak-key_twittertrend -Pcoverage'
                }

                echo "After withSonarQubeEnv block:"
                sh 'pwd'
            }
        } */
    }

    post {
        always {
            echo "This will run always, regardless of the build result"
        }
        success {
            echo "This will run only if the build is successful"
        }
        failure {
            echo "This will run only if the build fails"
        }
    }
}
