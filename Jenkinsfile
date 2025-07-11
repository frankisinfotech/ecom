pipeline {
  agent any

  environment {
    REPOSITORY_TAG = 'frankisinfotech/reactapp:${BUILD_ID}'
  }
 
 
  stages {

     stage ('BuildImg') {
      steps {
        withDockerRegistry([credentialsId: "DOCKERHUB", url: ""]) {
          sh '''
              docker build -t reactimg .
              docker tag reactimg frankisinfotech/reactapp:${BUILD_ID}
              docker push frankisinfotech/reactapp:${BUILD_ID}
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
