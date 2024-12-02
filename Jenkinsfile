pipeline {
  agent any
  tools {
    maven 'M3'
    jdk 'jdk17'
  }

  stages {
    stage('Git clone') {
      steps {
        git url: 'https://github.com/jdw0309/spring-petclinic.git',
        branch: 'main'
      }        
  }
  stage('Maven Build') {
    steps {
      sh 'mvn -Dmaven.test.failure.ignore=true clean package'
    }
  }
  stage('Docker Image') {
    steps {
      dir("${env.WORKSPACE}") {
       sh """
        docker build -t dowon113/spring-petclinic:$BUILD_NUMBER .
        docker tag dowon113/spring-petclinic:$BUILD_NUMBER dowon113/spring-petclinic:latest
       """
       }
      }
    }
        
  }
}
