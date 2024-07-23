pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo 'Checking out source code...'
                    checkout scm
                }
            }
        }

        stage('Verify docker-compose.yml') {
            steps {
                script {
                    echo 'Verifying docker-compose.yml file...'
                    sh 'ls -l'
                    sh 'cat docker-compose.yml'
                }
            }
        }

        stage('Build and Test Frontend') {
            steps {
                dir('frontend') {
                    script {
                        echo 'Building frontend Docker image...'
                        try {
                            sh 'sudo docker build -t frontend-image .'
                            // Optional: Run tests
                            // sh 'docker run --rm frontend-image npm test'
                        } catch (e) {
                            echo "Build and Test Frontend failed: ${e}"
                            error "Build and Test Frontend failed"
                        }
                    }
                }
            }
        }

        // ... other stages ...

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying services using Docker Compose...'
                    try {
                        sh 'sudo docker-compose -f docker-compose.yml up -d'
                    } catch (e) {
                        echo "Deployment failed: ${e}"
                        error "Deployment failed"
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Cleaning up Docker system...'
                try {
                    sh 'sudo docker system prune -f'
                } catch (e) {
                    echo "Cleanup failed: ${e}"
                }
            }
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
