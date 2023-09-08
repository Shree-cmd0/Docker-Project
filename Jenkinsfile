pipeline {

  environment {
    registry = "gmkmukesh333/flask"
    registry_mysql = "gmkmukesh333/mysql"
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
          withDockerRegistry([ credentialsId: "gmkmukesh333-DockerHub", url: "" ]) {
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
       withDockerRegistry([ credentialsId: "gmkmukesh333-DockerHub", url: "" ]) {
       sh 'docker build -t "gmkmukesh333/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
       
       sh 'docker push "gmkmukesh333/mysql:$BUILD_NUMBER"'
        }
      }}}
      
    stage('Push MySQL Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "gmkmukesh333-DockerHub", url: "" ]) {
            dockerImage.push("registry_mysql")
          }
        }
      }
    }
    //stage('Push MySQL Image') {
    //  steps{
    //    script {
    //      withDockerRegistry([ credentialsId: "gmkmukesh333-DockerHub", url: "" ]) {
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
