pipeline {
    agent any 

    stages {
        stage('Build') {
            steps {
                echo 'Building Docker Image...'
                // This builds your image and tags it
                sh 'docker build -t localhost:5000/hello-arch:v1 .'
            }
        }
        stage('Push') {
            steps {
                echo 'Pushing to Local Registry...'
                // This sends the image to your k3d registry
                sh 'docker push localhost:5000/hello-arch:v1'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying to Kubernetes...'
                // This tells Kubernetes to run your image
                sh 'kubectl create deployment hello-arch --image=k3d-my-registry:5000/hello-arch:v1 || kubectl set image deployment/hello-arch hello-arch=k3d-my-registry:5000/hello-arch:v1'
            }
        }
    }
}

