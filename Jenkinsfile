pipeline {
  agent {
    docker {
      image 'docker:20.10'   // Docker CLI image
      args  '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  environment {
       imagename = "trippy7/goldenimage"
       registryCredential = 'docker-access'
       dockerImage = ''
           }

  
  stages {

    stage ('Build') {
      steps {
        sh 'mvn clean package'
        echo 'Maven Build'
      }
    }

    stage('Build Test') {
            steps {
                sh 'mvn test'
                echo 'Test Analysis'
            }
        }
    
      stage('Build Docker image') { 
          steps{
                script {
                   dockerImage = docker.build imagename + ":$BUILD_NUMBER"
                          } 
                      }
                }

     stage('Push Docker Image to DockerHub') {
           steps{
               script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                                              }
                                    }
                             }
                  }
     stage('Remove Unused docker image') {
          steps{
              sh "docker rmi $imagename:$BUILD_NUMBER"
                        }
            }  
   }
}
