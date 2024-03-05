pipeline {
    environment {
        dockerimagename = "abhibhardwaj4907/nodeapp1"
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
                        dockerImage.push("v1")
                    }
                }
            }
        }
        stage("Deploy to Kubernetes") {
            steps {
             withKubeConfig([credentialsId:'kuber-conf', serverUrl: 'https://10.0.0.100:6443']) {
                sh "kubectl apply -f deploymentservice.yml"
             }
            }
        }
    }
}
