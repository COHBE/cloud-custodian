pipeline {
    agent any
    environment {
        IMAGE = "cloudcustodian/c7n"
        IMAGE_VERSION = "latest"
        IMAGE_TAG = "$IMAGE:$IMAGE_VERSION"
        NEXUS_URL = "nexus.internal.connectforhealthco.com:8082"
    }

    stages {
        stage('Set Build Information') {
           steps {
             buildName "${BUILD_NUMBER}"
             buildDescription "tag: ${IMAGE_TAG}"
           }
        }
        stage('build Image') {

            steps {
                    script {
                       dockerImage = docker.build("${IMAGE_TAG}  ${BUILD_ARGS}")
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