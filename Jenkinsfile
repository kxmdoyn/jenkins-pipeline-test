pipeline {
    agent any

    environment {
        REGISTRY = "amdp-registry.skala-ai.com"
        PROJECT = "skala26a-ai2"
        IMAGE_NAME = "jenkins-pipeline-test"
        IMAGE = "${REGISTRY}/${PROJECT}/${IMAGE_NAME}"
        TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t ${IMAGE}:${TAG} .'
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push ${IMAGE}:${TAG}'
            }
        }
    }
}
