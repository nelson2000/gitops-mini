// node {
//     def app

//     stage('Clone repository') {
      

//         checkout scm
//     }

//     stage('Build image') {
  
//        app = docker.build("raj80dockerid/test")
//     }

//     stage('Test image') {
  

//         app.inside {
//             sh 'echo "Tests passed"'
//         }
//     }

//     stage('Push image') {
        
//         docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
//             app.push("${env.BUILD_NUMBER}")
//         }
//     }
    
//     stage('Trigger ManifestUpdate') {
//                 echo "triggering updatemanifestjob"
//                 build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
//         }
// }


pipeline {
    agent any

    stages {

        stage ('build image') {

            steps {
                echo 'Hello, '

                sh '''#!/bin/bash
                    cd /home/jenkins/jenkins/gitops-mini/
                        az acr login
                        docker build  -it gitops:1.0 .
                '''
            }
        }

        stage('Test Image') {
            
                echo "Image tested and passed"
            
        } 

        stage('Tag Image') {
            
              sh ''' 

                echo 'retag the image for the final push'

                docker tag gitops:1.0 thanosbranch.azurecr.io/gitops:1.0

              '''
            
        }

        stage('Push Image') {
            
        sh ''' 
            docker push thanosbranch.azurecr.io/gitops:1.0

        '''
            
        }

    }
}
