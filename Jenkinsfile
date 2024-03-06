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
             withKubeConfig([credentialsId:'kuber-conf', serverUrl: 'https://10.0.0.100:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
']) {
                 sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'  
                 sh 'chmod u+x ./kubectl'  
                 sh "./kubectl apply -f deploymentservice.yml"
             }
            }
        }
    }
}
