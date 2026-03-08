pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'vivekxdevv/jen-x-doc'
    }
    stages {
        stage('Fetch Code') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} ."
                sh "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }
        stage('Login & Push') {
            steps {
                // This ID must match the Credential ID you set in Step 1
                withCredentials([usernamePassword(credentialsId: 'vivek11', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    // Added a backslash before $PASS and $USER
                    sh "echo \$PASS | docker login -u \$USER --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:${env.BUILD_ID}"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }
        stage('Deploy Container') {
            steps {
                // Stop old container if exists, and run the new one
                sh "docker stop my-web-app || true"
                sh "docker rm my-web-app || true"
                sh "docker run -d -p 80:80 --name my-web-app ${DOCKER_IMAGE}:latest"
            }
        }
    }
}

