pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Building the alpine version
                sh 'docker build -t k3d-dev-cluster-registry:5000/hello-alpine:v1 .'
            }
        }
        stage('Push') {
            steps {
                // Pushing to your local k3d registry
                sh 'docker push k3d-dev-cluster-registry:5000/hello-alpine:v1'
            }
        }
        stage('Deploy') {
            steps {
                // Running it in Kubernetes
                sh 'kubectl run hello-web --image=k3d-dev-cluster-registry:5000/hello-alpine:v1 --port=80 --overrides=\'{"spec": {"containers": [{"name": "hello-web", "image": "k3d-dev-cluster-registry:5000/hello-alpine:v1", "ports": [{"containerPort": 80}]}]}}\' || true'
            }
        }
    }
}