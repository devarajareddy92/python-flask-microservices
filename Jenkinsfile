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

        stage('Build and Test Order Service') {
            steps {
                dir('order-service') {
                    script {
                        echo 'Building order service Docker image...'
                        try {
                            sh 'sudo docker build -t order-service-image .'
                            // Optional: Run tests
                            // sh 'docker run --rm order-service-image ./run-tests.sh'
                        } catch (e) {
                            echo "Build and Test Order Service failed: ${e}"
                            error "Build and Test Order Service failed"
                        }
                    }
                }
            }
        }

        stage('Build and Test Product Service') {
            steps {
                dir('product-service') {
                    script {
                        echo 'Building product service Docker image...'
                        try {
                            sh 'sudo docker build -t product-service-image .'
                            // Optional: Run tests
                            // sh 'docker run --rm product-service-image ./run-tests.sh'
                        } catch (e) {
                            echo "Build and Test Product Service failed: ${e}"
                            error "Build and Test Product Service failed"
                        }
                    }
                }
            }
        }

        stage('Build and Test User Service') {
            steps {
                dir('user-service') {
                    script {
                        echo 'Building user service Docker image...'
                        try {
                            sh 'sudo docker build -t user-service-image .'
                            // Optional: Run tests
                            // sh 'docker run --rm user-service-image ./run-tests.sh'
                        } catch (e) {
                            echo "Build and Test User Service failed: ${e}"
                            error "Build and Test User Service failed"
                        }
                    }
                }
            }
        }

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
