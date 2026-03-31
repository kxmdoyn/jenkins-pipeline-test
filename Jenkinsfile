pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo '빌드 수행'
                sh 'echo "build start"'
            }
        }

        stage('Test') {
            steps {
                echo '테스트 수행'
                sh 'echo "test start"'
            }
        }

        stage('Deploy') {
            steps {
                echo '배포 수행'
                sh 'echo "deploy start"'
            }
        }
    }
}
