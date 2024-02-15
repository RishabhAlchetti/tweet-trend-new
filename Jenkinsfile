pipeline {
    agent {
        node {
          label 'maven'    
        }
    }
   environment{
    PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
}     

    stages {
        stage("build"){
          steps{
            echo "------------ build started -----------"
            sh 'mvn clean deploy -Dmaven.test.skip=true'
            echo "------------ build completed -----------"
          }
        }
        stage("test"){
           steps{
             echo "--------------unit test started -----------"
             sh 'mvn surefire-report:report'
             echo "--------------unit test completed -----------"
           }
        }
       stage('SonarQube analysis') {
            environment {   
                scannerHome = tool 'shak-sonar-scanner'
            }
            steps {
                script {
                    catchError(buildResult: 'SUCCESS') {
                        timeout(time: 2, unit: 'MINUTES') {
                        withSonarQubeEnv('shak-sonarqube-server') {
                            sh "${scannerHome}/bin/sonar-scanner"
                        }
                     }     
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
                script {
                    timeout(time: 2, unit: 'Minutes') {
                        // Just in case something goes wrong, pipeline will be killed after a timeout
                        def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
    }
}

   
