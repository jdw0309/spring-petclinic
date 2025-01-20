pipeline {
  agent any
  tools {
    maven 'M3'
    jdk 'JDK17'
  }

//Docker Hub 접속 정보
  environment{
    DOCKERHUB_CREDENTIALS = credentials('dockerCredentials')
    AWS_CREDENTIALS = credentials('dockerCredentials')
    GIT_CREDENTIALS = credentials('dockerCredentials')
    REGION = 'ap-northeast-2'
  }
  stages {
    // GitHub에 가서 소스코드 가져오기
    stage('Git Clone') {
      steps {
        echo 'Git Clone'
        git url: 'https://github.com/jdw0309/spring-petclinic.git'
          branch: 'main', credentialIdl: 'GIT_CREDENTIALS'
      }
    } 
  }
}
