pipeline {
  environment {
    registry = "vasylkosiv/go-app"
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
        sh "docker build -t $registry:$BUILD_NUMBER ."
      }
    }
    stage('Run container') {
      steps{
        sh "docker run $registry:$BUILD_NUMBER"
      }
    }
    stage('Deploy Image') {
      environment {
        registryCredential = credentials('docker-creds')
      }
      steps{
        
        sh "docker login -u vasylkosiv -p '$registryCredential'"
        sh "docker push $registry:$BUILD_NUMBER" 
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
