pipeline {
  agent any

  stages {

    stage (buildimg) {
      steps {
        withDockerRegistry([credentialsId: 'DOCKERHUB_USERNAME', url: ""]) {
          sh '''
            docker build -t ecomimg .
            docker tag ecomimg frankisinfotech/ecomimg
            docker push frankisinfotech/ecomimg
          '''
        }
      }
    }

  }
}
