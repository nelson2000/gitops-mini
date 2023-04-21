

pipeline{
  agent any
  tools{
    maven "maven3.9"
  }
  stages{

    stage("Build Image"){
      steps{
        echo "Building images ...."
        sh "cd /home/jenkins/jenkins/gitops-mini/"
        sh "az acr login thanosbranch"
        sh "docker build -it gitops:1.0 . "
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
        sh "docker tag gitops:1.0 thanosbranch.azurecr.io/gitops:1.0"
      }
    }
    stage("Push Image"){
      steps{
            sh "docker push thanosbranch.azurecr.io/gitops:1.0"
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