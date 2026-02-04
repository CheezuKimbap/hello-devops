pipeline {
    agent any

    stages {
       stage('Build') {
            agent {
                docker {
                    image 'docker:cli'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                echo 'Building Docker Image...'
                sh 'docker build -t localhost:5000/hello-arch:v1 .'
            }
        }


        stage('Deploy to K8s') {
            agent {
                docker { image 'bitnami/kubectl:latest' }
            }
            steps {
                // We point to the internal k3d proxy name shown in your docker ps
                sh 'kubectl config set-cluster dev --server=https://k3d-dev-cluster-serverlb:6443 --insecure-skip-tls-verify=true'
                sh 'kubectl config set-context dev --cluster=dev'
                sh 'kubectl config use-context dev'
                sh 'kubectl run hello-web --image=k3d-dev-cluster-registry:5000/hello-alpine:v1 --port=80'
            }
        }
    }
}