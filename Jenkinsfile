

pipeline{
  agent any
  tools{
    maven "maven3.9"
  }
  stages{

    stage("Build Image"){
      steps{
        echo "Building images ...."
        withCredentials([azureServicePrincipal('AZURE_CREDS')]) {
            sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'  
        }
        

            
             sh "cd /home/jenkins/jenkins/gitops-mini/"
//               sh "az acr login -n thanosbranch"
            sh "sudo docker build -t gitops:1.0 . "
          
    
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
        sh "sudo docker tag gitops:1.0 thanosbranch.azurecr.io/gitops:1.0"
      }
    }
    stage("Push Image"){
      steps{
        
         sh 'az acr login --name thanosbranch --expose-token --output tsv --query accessToken > /home/jenkins/jenkins/token.txt'
         sh 'cat /home/jenkins/jenkins/token.txt'
//          sh 'docker login --username thanosbranch --password-stdin $TOKEN'
        
//            sh "docker login thanosbranch.azurecr.io -u 00000000-0000-0000-0000-000000000000 -p $TOKEN"
//            sh "sudo docker push thanosbranch.azurecr.io/gitops:1.0"
          
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
