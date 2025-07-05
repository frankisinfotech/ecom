pipeline {
  agent any

  // environment {
  //   SERVICE_NAME = "ecom"
  //   ORGANIZATION_NAME = "frankisinfotech"
  //   DOCKERHUB_USERNAME = "frankisinfotech"
  //   REPOSITORY_TAG = "${DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
  //   }
 
  stages {

    stage (buildimg_to_pub_ECR) {
      steps {
        withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
          sh '''
            docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/g0b5g9q2
            docker build -t reactapp .
            docker tag reactapp:latest public.ecr.aws/g0b5g9q2/reactapp:latest
            docker push public.ecr.aws/g0b5g9q2/reactapp:latest
          '''
        }
      }
    }

  }
}
