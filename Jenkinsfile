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
   /* stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }*/
    stage ('Docker Build') {
      steps {
       script {
          withDockerRegistry([credentialsId: 'registryCredential', url: "https://harbor.smartdev.vn/"]) {
            // we give the image the same version as the .war package
            def image = docker.build("registry:${BUILD_NUMBER}")
            image.push()
      }
    }
  }
}
 /*  stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    } */
    stage('Remove Unused docker image && run kubectl') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
        sh "/home/vuong/Documents/kubernetes-course-master/jenkins/kubectl.sh" 
      }
    }
  }
}
