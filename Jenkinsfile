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
        stage('checkout') {
            steps {
            git branch: 'main', url: 'https://github.com/RishabhAlchetti/tweet-trend-new.git'
            } 
        }
      
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
    }
}
   
