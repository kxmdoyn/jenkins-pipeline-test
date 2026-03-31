pipeline {
    agent any

    environment {
        REGISTRY = "amdp-registry.skala-ai.com"
        PROJECT = "skala25a"
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

        stage('Harbor Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'harbor-creds',
                        usernameVariable: 'HARBOR_USER',
                        passwordVariable: 'HARBOR_PASS'
                    )
                ]) {
                    sh '''
                    echo "$HARBOR_PASS" | docker login amdp-registry.skala-ai.com \
                    -u "$HARBOR_USER" \
                    --password-stdin
                    '''
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push ${IMAGE}:${TAG}'
            }
        }
    }
}
