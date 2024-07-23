pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Build and Test Frontend') {
            steps {
                dir('frontend') {
                    script {
                        try {
                            // Build Docker image for frontend
                            sh 'sudo docker build -t frontend-image .'
                            // Optional: Run tests
                            // sh 'docker run --rm frontend-image npm test'
                        } catch (e) {
                            error "Build and Test Frontend failed: ${e}"
                        }
                    }
                }
            }
        }

        stage('Build and Test Order Service') {
            steps {
                dir('order-service') {
                    script {
                        try {
                            // Build Docker image for order service
                            sh 'sudo docker build -t order-service-image .'
                            // Optional: Run tests
                            // sh 'docker run --rm order-service-image ./run-tests.sh'
                        } catch (e) {
                            error "Build and Test Order Service failed: ${e}"
                        }
                    }
                }
            }
        }

        stage('Build and Test Product Service') {
            steps {
                dir('product-service') {
                    script {
                        try {
                            // Build Docker image for product service
                            sh 'sudo docker build -t product-service-image .'
                            // Optional: Run tests
                            // sh 'docker run --rm product-service-image ./run-tests.sh'
                        } catch (e) {
                            error "Build and Test Product Service failed: ${e}"
                        }
                    }
                }
            }
        }

        stage('Build and Test User Service') {
            steps {
                dir('user-service') {
                    script {
                        try {
                            // Build Docker image for user service
                            sh 'sudo docker build -t user-service-image .'
                            // Optional: Run tests
                            // sh 'docker run --rm user-service-image ./run-tests.sh'
                        } catch (e) {
                            error "Build and Test User Service failed: ${e}"
                        }
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    try {
                        // Deploy all services using Docker Compose
                        sh 'sudo docker-compose -f docker-compose.yml up -d'
                    } catch (e) {
                        error "Deployment failed: ${e}"
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker containers and images if needed
            script {
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
