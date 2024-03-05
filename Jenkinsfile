pipeline {
    environment {
        dockerimagename = "abhibhardwaj4907/nodeapp"
        dockerImage = ""
        registryCredentials = "dockerhub-creds"
    }
    agent any

    stages {
        stage("Checkout Git") {
            steps {
                git "https://github.com/arytmw/nodeapp_test.git"
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
             withKubeConfig([credentialsId: 'networknuts-k8s', serverUrl: 'https://networknutsk8s-dns-kvp2667g.hcp.centralindia.azmk8s.io']) {
                sh "kubectl apply -f deploymentservice.yml"
             }
            }
        }
    }
}
