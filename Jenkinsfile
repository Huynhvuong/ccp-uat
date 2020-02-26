pipeline {
  environment {
    registry = 'library/ccp-uat'
    tag_beta = "${currentBuild.displayName}-${env.BRANCH_NAME}"
  }
  agent {
    docker {
      image ''
      registryUrl 'https://harbor.smartdev.vn'
      registryCredentialsId 'harbor'
    }
  }
  triggers {
         pollSCM('* * * * *')
  }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/Huynhvuong/ccp-uat.git'
      }
    }  
    stage ('Docker Build') {
      steps {
       script {
         def image = docker.build("${env.registry}:${env.tag_beta}")
            image.push()
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
