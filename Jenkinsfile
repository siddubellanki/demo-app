pipeline {
    agent any

    environment {
        IMAGE = "cddu123/demo-app"  // Docker Hub image name
    }

    stages {

        // Step 1: Clone Git repository
        stage('Clone Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/siddubellanki/demo-app.git'
            }
        }

        // Step 2: Build Docker image
        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE:latest .'
            }
        }

        // Step 3: Push Docker image to Docker Hub
        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',  // Jenkins credentials ID
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                      docker push $IMAGE:latest
                    '''
                }
            }
        }

        // Step 4: Deploy to Kubernetes
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }

    }
}

