pipeline {

  environment {
    registry = "subhasree02/flask"
    registry_mysql = "subhasree02/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/Shree-cmd0/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ""
        }
      }
    }

    stage('Push Flask Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "dockerhub-id", url: "" ]) {
            dockerImage.push("registry")        
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
       withDockerRegistry([ credentialsId: "dockerhub-id", url: "" ]) {
       sh 'docker build -t "subhasree02/mysql"  "$WORKSPACE"/mysql'
       
       sh 'docker push "Shree-cmd0/mysql"'
        }
      }}}
      
    stage('Push MySQL Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "dockerhub-id", url: "" ]) {
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
          withKubeConfig([credentialsId: 'kube']) {
          sh 'kubectl apply -f frontend.yaml'
        }
      }
    }

  }
    }
}
