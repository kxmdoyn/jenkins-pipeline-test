pipeline {
    agent any

    environment {
        REGISTRY = "amdp-registry.skala-ai.com"
        PROJECT = "skala26a-ai2"
        IMAGE_NAME = "jenkins-pipeline-test"
        IMAGE = "${REGISTRY}/${PROJECT}/${IMAGE_NAME}"
        TAG = "${BUILD_NUMBER}"
        HARBOR_USER = 'robot$skala26a-ai2'
        K8S_NAMESPACE = "class-2"
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
                withCredentials([string(credentialsId: 'harbor-ai2-token', variable: 'HARBOR_TOKEN')]) {
                    sh '''
                        docker logout amdp-registry.skala-ai.com || true
                        echo ${HARBOR_TOKEN} | docker login amdp-registry.skala-ai.com \
                          -u ${HARBOR_USER} \
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

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    cp deployment.yaml deployment-rendered.yaml
                    sed -i "s|__IMAGE__|${IMAGE}:${TAG}|g" deployment-rendered.yaml
                    kubectl apply -f deployment-rendered.yaml -n ${K8S_NAMESPACE}
                    kubectl apply -f service.yaml -n ${K8S_NAMESPACE}
                '''
            }
        }

        stage('Check Rollout Status') {
            steps {
                sh '''
                    kubectl rollout status deployment/${IMAGE_NAME} -n ${K8S_NAMESPACE} --timeout=300s
                    kubectl get pods -n ${K8S_NAMESPACE}
                '''
            }
        }
    }
}
