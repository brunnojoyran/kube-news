pipeline {
    agent any

    stages {

        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("joyran/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

         stage ('Push DOcker Image') {
             steps {
                 script {
                     docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                     dockerapp.push('latest')
                     dockerapp.push("${env.BUILD_ID}")
                }
            }
        
        }
    }
        stage ('Deploy Kubernetes') {
            steps {
                withkubeConfig ([credentialsId: 'kube_config']) {
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }
    }

}