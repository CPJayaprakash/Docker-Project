pipeline {

  environment {
    registry = "10.202.0.3:5001/CPJayaprakash/flask"
    registry_mysql = "10.202.0.3:5001/CPJayaprakash/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/CPJayaprakash/Docker-Project.git'
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
       sh 'sudo docker build .-t "10.202.0.3:5001/CPJayaprakash/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'sudo docker push "10.202.0.3:5001/CPJayaprakash/mysql:$BUILD_NUMBER"'
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
