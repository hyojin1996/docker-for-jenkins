pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hyosim1996/jenkins-docker-example:latest'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Test Docker Command') {
            steps {
                echo 'Testing Docker command...'
                sh 'docker run --rm $DOCKER_IMAGE'
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image to Docker Hub...'
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
    }
}
