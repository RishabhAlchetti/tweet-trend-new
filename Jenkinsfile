pipeline {
    agent {
        node {
          label 'maven'    
        }
    }

    stages {
        stage('checkout') {
            steps {
            git branch: 'min', url: 'https://github.com/RishabhAlchetti/tweet-trend-new.git'
            }
        }
    }
}
