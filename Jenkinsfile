pipeline {
    agent any

    environment {
        IMAGE = "jenkins-app:build-${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t ${IMAGE} .'
            }
        }

        stage('Deploy & Verify') {
            steps {
                sh '''
                    docker rm -f jenkins-app || true
                    docker run -d --name jenkins-app -p 8081:80 ${IMAGE}

                    sleep 3

                    code=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8081)

                    echo "HTTP: $code"

                    test "$code" = "200"
                '''
            }
        }
    }
}
