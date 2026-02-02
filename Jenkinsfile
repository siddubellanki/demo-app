pipeline {
    agent any

    environment {
        IMAGE = "cddu123/demo-app"
    }

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE:latest .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
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

        stage('Deploy') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}

