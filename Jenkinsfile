pipeline {
  environment {
    registry = "harbor.smartdev.vn/library/ccp-uat"
    registryCredential = 'harbor'
    dockerImage = ''
  }
  agent any
  triggers {
         pollSCM('* * * * *')
  }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/Huynhvuong/ccp-uat.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":latest"
        }
      }
    }
    stage('Push Image') {
      steps{
       // sh " docker login -u admin -p Harbor12345 https://harbor.smartdev.vn"
        sh " docker image push $registry:latest"
      }
    }
    stage('Remove Unused docker image && run kubectl') {
      steps{
        //sh "docker rmi $registry:$BUILD_NUMBER"
        sh "/home/vuong/Documents/kubernetes-course-master/jenkins/kubectl.sh" 
      }
    }
  }
}
