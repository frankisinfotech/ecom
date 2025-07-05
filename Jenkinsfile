pipeline {
  agent any

  environment {
    SERVICE_NAME = "ecom-reactapp"
    AWS_REPO = "public.ecr.aws/g0b5g9q2/reactapp"
    REPO = "public.ecr.aws/g0b5g9q2"
    DOCKERHUB_USERNAME = "frankisinfotech"
    REPOSITORY_TAG = "${DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
    }
  
  stages {

    stage (buildimg_to_pub_ECR) {
      steps {
        withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]){
          sh '''
            docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) ${REPO}
            docker build -t ${SERVICE_NAME} .
            docker tag ${SERVICE_NAME} ${AWS_REPO}:${BUILD_ID}
            docker push ${AWS_REPO}:${BUILD_ID}
          '''
        }
      }
    }

    stage (build_to_priv_ECR) {
      steps {
        sh '''
          docker login -u AWS -p $(aws ecr get-login-password --region eu-west-1) 765176032689.dkr.ecr.eu-west-1.amazonaws.com
          docker build -t merchantapi .
          docker tag merchantapi 765176032689.dkr.ecr.eu-west-1.amazonaws.com/merchantapi:${BUILD_ID}
          docker push 765176032689.dkr.ecr.eu-west-1.amazonaws.com/merchantapi:${BUILD_ID}
          '''
          }
        }

  }
} 
