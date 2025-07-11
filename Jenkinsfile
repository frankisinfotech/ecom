pipeline {
  agent any

  environment {
    
    SERVICE_NAME = "ecom"
    ORGANIZATION_NAME = "frankisinfotech"
    DOCKERHUB_USERNAME = "frankisinfotech"
    REPOSITORY_TAG = "${DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
    }
 
 
  stages {

     stage ('BuildImg') {
      steps {
        withDockerRegistry([credentialsId: "DOCKERHUB", url: ""]) {
          sh '''
              docker build -t reactimg .
              docker tag reactimg ${REPOSITORY_TAG}
              docker push ${REPOSITORY_TAG}
          '''
         }
       }
     }

    stage ("Install kubectl"){
            steps {
                sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl'
                sh 'chmod +x ./kubectl'
                sh './kubectl version --client'
            }
        }
        
    stage ('Deploy') {
            steps {
                sh 'aws eks update-kubeconfig --region eu-west-1 --name ekscluster'
                sh 'envsubst < ${WORKSPACE}/deploy.yaml | ./kubectl apply -f -'
            }
    }

    stage ('Delete Images') {
      steps {
        sh 'docker rmi -f $(docker images -q -a)'
      }
    }
   }
 }
