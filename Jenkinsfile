pipeline {
    agent any

    environment {
        // Replace with your Docker Hub username
        DOCKER_HUB_USERNAME = "salari111"
        // Name of the Docker image (repo name in Docker Hub)
        DOCKER_IMAGE_NAME = "my-custom-app"
        // Version tag
        DOCKER_IMAGE_TAG = "2.0"
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull code from your SCM (e.g., GitHub)
                checkout scm
            }
        }

        stage('Build Maven Project') {
            steps {
                dir('my-app') {
                    // Build the Maven project
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh """
                       docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    // Docker Hub credentials should be stored in Jenkins Credentials
                    // Go to Credentials -> System -> Global -> Add credentials
                    // Use an ID like "docker-hub-credentials"
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-credentials', 
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Tag and push to Docker Hub
                    sh """
                       docker tag ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ${DOCKER_HUB_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}
                       docker push ${DOCKER_HUB_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}
                    """
                }
            }
        }
    }

    post {
        always {
            // Optional: remove the local image to free space
            sh "docker rmi ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} || true"
            sh "docker rmi ${DOCKER_HUB_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} || true"
        }
    }
}
