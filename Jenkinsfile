pipeline {
  agent any
  tools {
    maven 'M3'
    jdk 'JDK17'
  }

//Docker Hub 접속 정보
  environment{
    DOCKERHUB_CREDENTIALS = credentials('dockerCredential')
    AWS_CREDENTIALS = credentials('AWSCredential')
    GIT_CREDENTIALS = credentials('gitCredential')
    REGION = 'ap-northeast-2'
  }
  stages {
    // GitHub에 가서 소스코드 가져오기
    stage('Git Clone') {
      steps {
        echo 'Git Clone'
        git url: 'https://github.com/jdw0309/spring-petclinic.git'
          branch: 'main', credentialsId: 'GIT_CREDENTIALS'
      }
    } 
  }
}
