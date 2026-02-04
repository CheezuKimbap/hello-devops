pipeline {
    agent any

    stages {
        stage('Build Image') {
            agent {
                docker {
                    image 'docker:cli'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t k3d-dev-cluster-registry:5000/hello-arch:v1 .'
            }
        }

        stage('Push Image') {
            agent {
                docker {
                    image 'docker:cli'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                echo 'Pushing Docker image...'
                sh 'docker push k3d-dev-cluster-registry:5000/hello-arch:v1'
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'bitnami/kubectl:latest'
                }
            }
            steps {
                sh '''
                kubectl config set-cluster dev \
                  --server=https://k3d-dev-cluster-serverlb:6443 \
                  --insecure-skip-tls-verify=true

                kubectl config set-context dev --cluster=dev
                kubectl config use-context dev

                kubectl apply -f k8s/deployment.yaml
                '''
            }
        }
    }
}
