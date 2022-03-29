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
           
            stage("Deploying")
                 {
                     steps
                     {
                         withKubeConfig([credentialsId: 'kubeconfig'])
                         {
                             sh 'pwd && ls'
                             sh 'kubectl apply -f minikube/mongodb.yml'
                             sh 'kubectl apply -f minikube/app.yml'
                             sh 'kubectl apply -f minikube/nodeport.yml'
                         }
                     }
                 }
      }
}
