pipeline {
  environment {
    registry = "pratush43/dock"
    registryCredential = 'dockerhub'
    image = '' 
  }
  agent none
    stages {
        stage('Build') {
          agent {
    node{
    label 'micro'
    } 
  }
            steps {
                sh 'dotnet build'
              sh ' ls -lrt && pwd'
              archiveArtifacts artifacts: 'bin/Debug/net6.0/*.dll'
              stash includes: 'bin/Debug/net6.0/*.dll', name: 'build', useDefaultExcludes: false
            }
        }
        stage("docker image"){
           agent any
      steps{
        script {
          unstash 'build'
         def dockerImage = docker.build registry + ":$BUILD_NUMBER"
          
      }

     
  }
    }
    stage('Deploy image'){
      agent {
        node{
       label 'deployment' 
        }
      }
      parameters {
  choice choices: ['Production environment', 'Test environment', 'Development environment'], description: 'This parameter selects the deployment environment', name: 'Choose build environment'
}
      steps{
        script{
                        
                   
        image = "$registry" + ":$BUILD_NUMBER"
            sh "docker run -d -p 8082:8081 '$image'"
      }
      }

    }
    }
    }
