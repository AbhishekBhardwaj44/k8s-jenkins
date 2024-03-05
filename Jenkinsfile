pipeline {
    environment {
        dockerimagename = "abhibhardwaj4907/nodeapp"
        dockerImage = ""
        registryCredentials = "dockerhub-cred"
    }
    agent any

    stages {
        stage("Checkout Git") {
            steps {
                checkout scm
            }
        }
        stage("Build Image") {
            steps {
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }
        stage("Push Image") {
            steps {
                script {
                    docker.withRegistry("", registryCredentials) {
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage("Deploy to Kubernetes") {
            steps {
             withKubeConfig([credentialsId: 'k8s-config', serverUrl: 'https://10.0.0.100:6443']) {
                sh "kubectl apply -f deploymentservice.yml
             }
            }
        }
    }
}
