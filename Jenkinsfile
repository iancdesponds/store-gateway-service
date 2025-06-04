pipeline {
    agent any
    environment {
        SERVICE = 'gateway'
        JOB_NAME = "store-${SERVICE}"
        IMAGE_NAME = "iancdesponds/${SERVICE}"
    }
    stages {
        stage('Build & Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credential', usernameVariable: 'USERNAME', passwordVariable: 'TOKEN')]) {
                    sh "echo $TOKEN | docker login -u $USERNAME --password-stdin"
                    sh "docker buildx create --use --platform=linux/arm64,linux/amd64 --node multi-platform-builder-${SERVICE} --name multi-platform-builder-${SERVICE}"
                    sh "docker buildx build --platform=linux/arm64,linux/amd64 --push --tag ${IMAGE_NAME}:latest --tag ${IMAGE_NAME}:${BUILD_ID} -f Dockerfile ."
                    sh "docker buildx rm --force multi-platform-builder-${SERVICE}"
                }
            }
        }
    }
}
