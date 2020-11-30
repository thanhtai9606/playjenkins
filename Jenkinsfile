pipeline {
    environment {
        registry = "ltdungc50/demo_cd"
        registryCredential = 'dockerhub_id'
        dockerImage = ''
        // BUILD_NUMBER = 'latest'
    }
    agent { label 'jenkins-linux' }
    stages {
        //     stage('Cloning our Git') {
        //     steps {
        //     git 'https://github.com/YourGithubAccount/YourGithubRepository.git'
        //     }
        // }
        stage('Building our image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
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
        stage('Cleaning up') {
            steps{
                    sh "docker rmi $registry:$BUILD_NUMBER"
                }
            }
        }      
        stage('Deploy App') {
            steps{
                script {
                    kubernetesDeploy(configs: "1.sample.cicd.yaml", kubeconfigId: "mykubeconfig")
                }
            }
        }
        
    // }
}
