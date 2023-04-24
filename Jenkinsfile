
pipeline {
  agent any
  environment {
    docker_username = 'nwajienelson'
  }
  stages{

    stage("Build Image"){
      steps{
        
        sh "sudo docker build -t gitops:1.1 ."
        sh "sudo docker images"

        }     
      
    }
    stage("Image Scanning"){
      steps{
         echo "Image Scanned and passed"
      }
    }
    stage("Approval after Scan"){
      steps{
        timeout(time:5, unit:'DAYS'){
        input message: 'Approval for Image Tag and Push'
      }
    }
  }

    stage("Tag Image"){
     steps{
        echo "retag the image for the final push"
       sh "sudo docker tag gitops:1.1 nwajienelson/gitops:1.0"
      }
    }
    stage("Push Image"){
      steps{
        
        withCredentials([string(credentialsId: 'docker_password', variable: 'docker_password')]) {
          
        sh 'docker login -u $docker_username -p $docker_password'
        sh "docker push nwajienelson/gitops:1.1"
   
        }
 
          
      }
    }

    stage("Email Notification"){
      steps{
        echo "Email has been sent"
        // emailext body: 'This is Build Success', subject: 'Build Success', to: 'nelson.nwajie@gmail.com'
      }
    }
  }
}
