pipeline {
    agent any

   // environment {
        // Define environment variables here if needed
    //}

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
                        // Build Docker image for frontend
                        sh 'sudo docker build -t frontend-image .'
                        // Optional: Run tests
                        // sh 'docker run --rm frontend-image npm test'
                    }
                }
            }
        }

        stage('Build and Test Order Service') {
            steps {
                dir('order-service') {
                    script {
                        // Build Docker image for order service
                        sh 'sudo docker build -t order-service-image .'
                        // Optional: Run tests
                        // sh 'docker run --rm order-service-image ./run-tests.sh'
                    }
                }
            }
        }

        stage('Build and Test Product Service') {
            steps {
                dir('product-service') {
                    script {
                        // Build Docker image for product service
                        sh 'sudo docker build -t product-service-image .'
                        // Optional: Run tests
                        // sh 'docker run --rm product-service-image ./run-tests.sh'
                    }
                }
            }
        }

        stage('Build and Test User Service') {
            steps {
                dir('user-service') {
                    script {
                        // Build Docker image for user service
                        sh 'sudo docker build -t user-service-image .'
                        // Optional: Run tests
                        // sh 'docker run --rm user-service-image ./run-tests.sh'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy all services using Docker Compose
                    sh 'sudo docker-compose -f docker-compose.yml up -d'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker containers and images if needed
            sh 'docker system prune -f'
        }
    }
}
