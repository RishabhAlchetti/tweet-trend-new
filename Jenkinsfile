pipeline {
    agent {
        node {
            label 'maven'
        }
    }
    stages{
        stage('checkout'){
         steps{
         git branch: 'main', url: 'https://github.com/RishabhAlchetti/tweet-trend-new.git'
         } 
       }
    }
}
