pipeline {
  agent any
  tools {
    maven 'M3'
    jdk 'jdk17'
  }

  stages {
    stage('Git clone') {
      steps {
        git url: 'https'://github.com/jdw0309/spring-petclinic.git',
        branch: 'main'
      }        
  }
}
}
