pipeline {
    agent none

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
                sh 'docker version'
                sh 'docker build -t k3d-dev-cluster-registry:5000/hello-arch:v1 .'
            }
        }
    }
}
