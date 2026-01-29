pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('dockerhub')
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/cddu123/demo-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t cddu123/demo-app:${BUILD_NUMBER} .
                docker tag cddu123/demo-app:${BUILD_NUMBER} cddu123/demo-app:latest
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                echo "$DOCKERHUB_PSW" | docker login -u "$DOCKERHUB_USR" --password-stdin
                docker push cddu123/demo-app:${BUILD_NUMBER}
                docker push cddu123/demo-app:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Pipeline Failed!"
        }
    }
}

