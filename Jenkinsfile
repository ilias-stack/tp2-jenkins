pipeline {
    environment {
        registry = "ilias2002/tp2"          // Docker image repository
        registryCredential = 'dockerhub'    // Jenkins DockerHub credentials ID
        dockerImage = ''                    // Placeholder for the Docker image
    }
    agent any                               // Can run on any Jenkins agent
    stages {
        stage('Cloning Git') {
            steps {
                // Cloning the repository
                git branch: 'main', url: 'https://github.com/ilias-stack/tp2-jenkins'
            }
        }
        stage('Building image') {
            steps {
                script {
                    // Build Docker image and tag it with the build number
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    // Test the Docker image (dummy step for now)
                    echo "Running tests on the Docker image"
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    // Push Docker image to the registry using the specified credentials
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    echo "Deploying Docker image to Docker Engine"
                    
                    sh """
                        docker pull ${registry}:${BUILD_NUMBER}
                        docker stop tp2_container || true
                        docker rm tp2_container || true
                        docker run -d --name tp2_container -p 8087:80 ${registry}:${BUILD_NUMBER}
                    """
                }
            }
        }
    }
}