def registry = 'https://rishabh06.jfrog.io/'
def imageName = 'rishabh06.jfrog.io/docker-repo-docker-local'
def version   = '2.1.3'

pipeline {
    agent {
        node {
            label 'Maven'
        }
    }
    environment {
        PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
    }
    stages {
        stage("Git Clone") {
            steps {
                echo "----------- Cloning repository ----------"
                git branch: 'main', url: 'https://github.com/RishabhAlchetti/tweet-trend-new.git'
            }
        }
        stage("Build") {
            steps {
                echo "----------- Build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "----------- Build completed ----------"
            }
        }
        stage("Test") {
            steps {
                echo "----------- Unit test started ----------"
                sh 'mvn surefire-report:report'
                echo "----------- Unit test completed ----------"
            }
        }
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'shak-sonar-scanner'
            }
            steps {
                withSonarQubeEnv('sonar-shak') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
        stage("Jar Publish") {
            steps {
                script {
                    echo '<--------------- Jar Publish Started --------------->'
                    echo "Git commit ID: ${env.GIT_COMMIT}"  // Print Git commit ID for debugging
                    def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "jfrog-cred"
                    def properties = "buildid=${env.BUILD_ID},commitid=${env.GIT_COMMIT}"  // Use env.GIT_COMMIT
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "jarstaging/(*)",
                                "target": "maven-repo-libs-release-local/{1}",
                                "flat": "false",
                                "props": "${properties}",
                                "exclusions": ["*.sha1", "*.md5"]
                            }
                        ]
                    }"""
                    def buildInfo = server.upload(uploadSpec)
                    buildInfo.env.collect()
                    server.publishBuildInfo(buildInfo)
                    echo '<--------------- Jar Publish Ended --------------->'  
                }
            }   
        } 
      stage(" Docker Build ") {
      steps {
        script {
           echo '<--------------- Docker Build Started --------------->'
           app = docker.build(imageName+":"+version)
           echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }
    
    stage("trivy ") {
      steps {
        script {
           echo '<--------------- Trivy Started --------------->'
           sh "trivy image ${imageName}:${version}"
           echo '<----------------- Trivy Ends --------------->'
        }
      }
    }
    

    stage (" Docker Publish "){
        steps {
            script {
               echo '<--------------- Docker Publish Started --------------->'  
                docker.withRegistry(registry, 'jfrog-cred'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
        }
    }  
        
    }
}
