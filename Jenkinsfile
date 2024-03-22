pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Define Docker build arguments
                    def dockerFile = './Dockerfile' // Path to your Dockerfile
                    def imageName = 'test'
                    def imageTag = 'latest'
                    echo "test valid"
                    // Docker build command
                    docker.build("${imageName}:${imageTag}", "-f ${dockerFile} .")
                }
            }
        }
    }
}
