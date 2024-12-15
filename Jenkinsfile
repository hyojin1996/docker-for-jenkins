pipeline {
    agent any

    environment {
        // Docker 이미지 이름 설정
        DOCKER_IMAGE = 'hyosim1996/jenkins-docker-example:latest'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from SCM...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo 'Running Docker container...'
                sh 'docker run --rm $DOCKER_IMAGE'
            }
        }
    }
}
