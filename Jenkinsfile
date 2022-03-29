pipeline{
    agent any
    environment { 
        registry = "yogesh0707/final_capstone" 
        registryCredential = 'yogesh' 
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
                        dockerImage = docker.build registry + ":$GIT_COMMIT-$BUILD_NUMBER"
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
            1
           stage("SSH Into k8s Server") {
                  def remote = [:]
                      remote.name = 'K8S master'
                          remote.host = '127.0.0.1'
                              remote.user = 'vagrant'
                                  remote.password = 'vagrant'
                                      remote.allowAnyHosts = true
            } 
      }
}
