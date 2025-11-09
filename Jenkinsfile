pipeline {
  agent any

  environment {
    
        TAG                       = "${BUILD_ID}"
        AWS_PRIVATE_REPO          = "765176032689.dkr.ecr.eu-west-1.amazonaws.com"
        AWS_APP_NAME              = "merchantpay"
        REPOSITORY_TAG            = "${AWS_PRIVATE_REPO}/${AWS_APP_NAME}:${TAG}"
    }
 
   
  stages {

     stage ('BuildImg') {
          steps {
             withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
                sh 'docker login -u AWS -p $(aws ecr get-login-password --region eu-west-1) ${AWS_PRIVATE_REPO}'
                sh 'docker build -t ${AWS_APP_NAME}:${TAG} .'
                sh 'docker tag ${AWS_APP_NAME}:${TAG} ${AWS_PRIVATE_REPO}/${AWS_APP_NAME}:${TAG}'
                sh 'docker push ${AWS_PRIVATE_REPO}/${AWS_APP_NAME}:${TAG}'
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
                sh 'aws eks update-kubeconfig --region eu-west-1 --name eks'
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
