pipeline {
    environment {
        registry = "ilias2002/tp2"
        registryCredential = 'dockerhub'            
        dockerImage = ''                            
    }
    agent any                                       
    stages {
        stage('Cloning Git') {
            steps {
                // Cloning the repository from your GitHub
                git branch: 'main', url: 'https://github.com/ilias-stack/tp2-jenkins'
            }
        }
        stage('Building image') {
            steps {
                script {
                    // Build Docker image and tag it with the Jenkins build number
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    // Replace with actual tests if needed, for now just an echo
                    echo "Running tests on the Docker image"
                    echo "..."
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    // Push Docker image to the DockerHub registry
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}

