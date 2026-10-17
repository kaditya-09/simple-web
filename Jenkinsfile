pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    environment {
        IMAGE_NAME = "kaditya09/simple-web"
        IMAGE_TAG  = "latest"
        DOCKER_HUB = credentials('dockerhub-creds')
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh 'echo $DOCKER_HUB_PSW | docker login -u $DOCKER_HUB_USR --password-stdin'
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f simple-web || true'
                sh 'docker run -d --name simple-web -p 8082:80 $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }
}
