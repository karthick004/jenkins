pipeline {
    agent any
  tools {
        dockerTool 'docker1'
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/your-repo/my-repo']]])
                sh 'ls -l' // List files in the workspace for debugging
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        // Build the Docker image from the Dockerfile in the 'docker/' directory
                        sh 'docker build -t myapp:latest docker/'
                    } catch (Exception e) {
                        echo "Failed to build Docker image: ${e}"
                        error("Docker build failed")
                    }
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    try {
                        // Run the Docker container
                        sh 'docker run -d --name myapp-container -p 3000:3000 myapp:latest'
                    } catch (Exception e) {
                        echo "Failed to run Docker container: ${e}"
                        error("Docker run failed")
                    }
                }
            }
        }
    }
    post {
        failure {
            echo "Pipeline failed! Check the logs for details."
        }
        success {
            echo "Pipeline completed successfully!"
        }
    }
}
