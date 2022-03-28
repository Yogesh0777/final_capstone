pipeline{
    agent any
    environment { 
        registry = "atishashaurya2865/final_capstone1" 
        registryCredential = 'atisha' 
        dockerImage = '' 
    }    
    tools { 
        maven 'maven3'
    }
    stages
       {
          stage("Build")
            {
                steps{
                    sh "mvn compile"
                }
                
            }
            stage("Test")
            {
                steps{
                    sh "mvn clean test"
                }
            }
            stage("Package")
            {
                steps{
                    sh "mvn clean package"
                }
            }
            stage('Building our image') { 
                steps { 
                    script { 
                        dockerImage = docker.build registry + ":$GIT_COMMIT-build-BUILD_NUMBER"
                    }
                } 
            }
            stage('Deploy our image') { 
                steps { 
                    script { 
                        docker.withRegistry( '', registryCredential ) { 
                            dockerImage.push() 
                        }
                    } 
                }
            }            
      }
}
