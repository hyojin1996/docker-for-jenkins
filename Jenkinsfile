pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hyosim1996/jenkins-docker-example:latest'
        DOCKER_CREDENTIALS = 'docker-hub-credentials' // Jenkins에 등록된 Docker Hub 인증 정보 ID
        KUBECONFIG = '/path/to/kubeconfig' // 쿠버네티스 접근용 kubeconfig 경로
        K8S_NAMESPACE = 'default' // Kubernetes 네임스페이스
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
                sh '''
                docker build -t $DOCKER_IMAGE .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: "$DOCKER_CREDENTIALS", usernameVariable: "DOCKER_USER", passwordVariable: "DOCKER_PASS")]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Run Integration Tests') {
            steps {
                echo 'Running integration tests...'
                sh '''
                docker run --rm --network -d -p 5050:5050 --name test-container $DOCKER_IMAGE
                sleep 20
                curl --retry 5 --retry-delay 5 http://localhost:5050/health | grep "OK"
                docker stop test-container
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Kubernetes...'
                sh '''
                kubectl --kubeconfig=$KUBECONFIG set image deployment/jenkins-app jenkins-app=$DOCKER_IMAGE --namespace=$K8S_NAMESPACE
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
