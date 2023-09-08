pipeline {

  environment {
    registry = "gmkmukesh333/flask"
    registry_mysql = "gmkmukesh333/mysql"
    dockerImage = ""
    //DOCKER_CREDENTIALS = credentials('gmkmukesh333-DockerHub') 

 DOCKER_USERNAME = "gmkmukesh333-DockerHub"
 DOCKER_PASSWORD = "dckr_pat_aV8zzV8GMB4JxvrwMTiVWWetiQo"
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/gmkmukesh/Docker-Project.git'
      }
    }
      stage('Docker Login') {
            steps {
                script {
                    // Define your Docker registry credentials as Jenkins credentials
                    withCredentials([usernamePassword(credentialsId: 'docker-registry-credentials', usernameVariable: 'gmkmukesh333-DockerHub', passwordVariable: dckr_pat_aV8zzV8GMB4JxvrwMTiVWWetiQo')]) {
                        
                        // Log in to the Docker registry
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD <https://hub.docker.com/>"
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
