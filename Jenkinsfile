pipeline {
  agent any
  // JAVA와 Maven Tool 등록
  tools {
    jdk 'JDK17'
    maven 'M3'
  }

  // Docker Hub 접속 정보
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerCredential')
    AWS_CREDENTIALS = credentials('AWSCredential')
    //GIT_CREDENTIALS = credentials('gitCredential')
    REGION = 'ap-northeast-2'
  }

  stages {
    // GitHub에 가서 소스코드 가져오기
    stage('Git Clone') {
      steps {
        echo 'Git Clone'
        git url: 'https://github.com/jdw0309/spring-petclinic.git',
          branch: 'main'
        
      }
    }
    //Maven 빌드 작업
    stage('Maven Build') {
      steps {
        echo 'Maven Build'
        sh 'mvn -Dmaven.test.failure.ignore=true clean package'
      }
    }
    // Docker Image 생성
    stage('Docker Image Build') {
      steps {
        echo 'Docker Image Build'
        dir("${env.WORKSPACE}") {
          sh """
          docker build -t dowon113/spring-petclinic:$BUILD_NUMBER .
          docker tag dowon113/spring-petclinic:$BUILD_NUMBER dowon113/spring-petclinic:latest
          """
        }
      }
    }

    // DockerHub Login
    stage('Docker Login') {
      steps {
        sh """
        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
        docker push dowon113/spring-petclinic:latest
        """
      }
    }
    
    // Docker Image 삭제
    stage('Remove Docker Image') {
      steps {
        sh """
        docker rmi dowon113/spring-petclinic:$BUILD_NUMBER
        docker rmi dowon113/spring-petclinic:latest
        """
      }
    }

   // S3에 Appspec.yml과 실행 스크립트 저장
        stage('Upload to S3') {
            steps {
                echo "Upload to S3"
                dir("${env.WORKSPACE}") {
                    sh 'zip -r deploy.zip ./deploy appspec.yml'
                    withAWS(region:"${REGION}", credentials:"${AWS_CREDENTIALS}"){
                      s3Upload(file:"deploy.zip", bucket:"user04-codedeploy-bucket")
                    } 
                    sh 'rm -rf ./deploy.zip'                 
                }        
            }
        }

    
  }
}
