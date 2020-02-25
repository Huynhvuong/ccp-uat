pipeline {
  environment {
    registry = "huynhvuong565/ccp-uat"
    registryCredential = '565'
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
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image && run kubectl') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
        sh "/home/vuong/Documents/kubernetes-course-master/jenkins/kubectl.sh" 
      }
    }
  }
}
