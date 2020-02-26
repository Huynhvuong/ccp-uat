pipeline {
  environment {
    registry = "harbor.smartdev.vn/library/ccp-uat"
    registryCredential = 'harbor'
    dockerImage = ''
  }
  parameters {
    string(name: 'rancher', defaultValue: '10.10.1.82', description: 'Rancher Server')
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
        sh "docker rmi $registry:latest"
        sh "/home/vuong/Documents/kubernetes-course-master/jenkins/kubectl.sh" 
        sh "ssh -i /home/vuong/.ssh/id_rsa -v -o StrictHostKeyChecking=no root@${params.rancher} 'kubectl create -f /home/smartdev/tst/vuong.yml'"
      }
    }
  }
}
