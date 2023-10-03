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
          dockerImage = docker.build registry + ""
        }
      }
    }

    stage('Push Flask Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "gmkmukesh333-DockerHub", url: "" ]) {
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
       withDockerRegistry([ credentialsId: "gmkmukesh333-DockerHub", url: "" ]) {
       sh 'docker build -t "gmkmukesh333/mysql"  "$WORKSPACE"/mysql'
       
       sh 'docker push "gmkmukesh333/mysql"'
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
          withKubeConfig([credentialsId: 'kube']) {
          sh 'kubectl apply -f frontend.yaml'
        }
      }
    }

  }
    }
}
