pipeline {
  agent any

  environment {
    SERVICE_NAME = "ecom"
    ORGANIZATION_NAME = "frankisinfotech"
    DOCKERHUB_USERNAME = "frankisinfotech"
    REPOSITORY_TAG = "${DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
    }
 
  stages {

    stage (buildimg) {
      steps {
        withDockerRegistry([credentialsId: 'DOCKERHUB_USERNAME', url: ""]) {
          sh '''
            docker build -t ${REPOSITORY_TAG} .
            docker push ${REPOSITORY_TAG}
          '''
        }
      }
    }

  }
}
