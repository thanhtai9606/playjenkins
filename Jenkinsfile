
pipeline {

  environment {
    registry = "ltdungc50/demo_cd"
    registryCredential = 'dockerhub_id'
    dockerImage = ""
  }

  agent { label 'jenkins-linux' }

  stages {
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
    stage('Deploy our image') {
            steps{
                script {
                        docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }
    stage('Cleaning up') {
        steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }      
  }

}
