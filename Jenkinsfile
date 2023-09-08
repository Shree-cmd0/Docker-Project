pipeline {

  environment {
    registry = "gmkmukesh333-DockerHub/flask"
    registry_mysql = "gmkmukesh333-DockerHub/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/gmkmukesh/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Flask Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "gmkmukesh333-DockerHub", url: "https://hub.docker.com" ]) {
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
        script { 
       withDockerRegistry([ credentialsId: "gmkmukesh333-DockerHub", url: "https://hub.docker.com" ]) {
       sh 'docker build -t "gopisuria/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
       
       sh 'docker push "gmkmukesh333/mysql:$BUILD_NUMBER"'
        }
      }}}
      
    stage('Push MySQL Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "gmkmukesh333-DockerHub", url: "https://hub.docker.com" ]) {
            dockerImage.push("registry_mysql")
          }
        }
      }
    }
    //stage('Push MySQL Image') {
    //  steps{
    //    script {
    //      withDockerRegistry([ credentialsId: "gmkmukesh333-DockerHub", url: "https://hub.docker.com" ]) {
    //        dockerImage.push('registry_mysql',)        
     //      }
     //   }
     // }
    // }
    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
