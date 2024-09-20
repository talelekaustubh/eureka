pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials' // Docker Hub credentials stored in Jenkins
        DOCKER_IMAGE = 'nodeapp'                         // Name of the Docker image
        DOCKER_TAG = 'latest'                            // Tag for the Docker image
        DOCKER_REGISTRY = 'docker.io'                     // Docker Hub registry URL
        REPO_NAME = 'sksupremeboss/nodeapp'               // Docker Hub repository name
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image and tag it
                    def customImage = docker.build("${REPO_NAME}:${DOCKER_TAG}")
                }
            }
        }

        stage('Push Docker Image to Registry') {
            steps {
                script {
                    // Log in to Docker Hub and push the image
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${REPO_NAME}:${DOCKER_TAG}").push()
                    }
                }
            }
        }
    }

    post {
        success {
            emailext(
                to: 'kaustubhtu@gmail.com',
                subject: 'Build Successful: ${env.JOB_NAME} [${env.BUILD_NUMBER}]',
                body: 'The Docker image has been built and pushed successfully.'
            )
        }
        failure {
            emailext(
                to: 'kaustubhtu@gmail.com',
                subject: 'Build Failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]',
                body: 'The Docker image build or push has failed. Please check the logs.'
            )
        }
    }
}
