pipeline {

  environment {
    registry = "gmkmukesh333/flask"
    registry_mysql = "gmkmukesh333/mysql"
    dockerImage = ""
    DOCKER_CREDENTIALS = credentials('gmkmukesh333-DockerHub') 

  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/mgsgoms/Docker-Project.git'
      }
    }
stage('Build and Push Image') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com/', 'gmkmukesh333-DockerHub') {
                        
                    }
                }
            }
        }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
   stage('Build mysql image') {
     steps{
       sh 'docker build -t "gmkmukesh333/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "gmkmukesh333/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
