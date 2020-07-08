pipeline {
  environment {
    registry = "vasylkosiv/go-app"
    registryCredential = 'docker-creds'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Pulling from Git') {
      steps {
        git 'https://github.com/VasylKosiv/test.git'
      }
    }
    stage('Building docker image') {
      steps{
        sh "docker build -t $registry:$BUILD_NUMBER"
      }
    }
    stage('Run container') {
      steps{
        sh "docker run $registry:$BUILD_NUMBER"
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
