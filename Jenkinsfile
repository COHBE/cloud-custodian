pipeline {
    agent any
    environment {
        IMAGE = "cloudcustodian/c7n"
        IMAGE_VERSION = "latest"
        IMAGE_TAG = "$IMAGE:$IMAGE_VERSION"
        NEXUS_URL = "nexus.internal.connectforhealthco.com:8082"
    }

    stages {
        stage('build Image') {
            agent {
                docker {
                    image 'ubuntu:20.04'
                    reuseNode true
                }
            }
            steps {
                    script {
                        dockerImage = docker.build("${IMAGE_TAG}")
                    }
            }
        }
        stage('tag Image') {

            steps {
                   sh "docker tag ${IMAGE_TAG} ${NEXUS_URL}/${IMAGE_TAG}"
            }
        }

        stage('Push to Nexus') {
            steps {
                sh "docker push ${NEXUS_URL}/${IMAGE_TAG}"
            }
        }
    }
}