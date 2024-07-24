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
        stage('Create Docker Network') {
            steps {
                script {
                    echo 'Creating Docker network...'
                    try {
                        sh 'sudo docker network create micro_network || true'
                    } catch (e) {
                        echo "Network creation failed: ${e}"
                        error "Network creation failed"
                    }
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
                            // sh 'sudo docker run --rm frontend-image npm test'
                        } catch (e) {
                            echo "Build and Test Frontend failed: ${e}"
                            error "Build and Test Frontend failed"
                        }
                    }
                }
            }
        }

        stage('Deploy Frontend') {
            steps {
                dir('frontend') {
                    script {
                        echo 'Deploying frontend using Docker Compose...'
                        try {
                            sh 'sudo docker-compose -f docker-compose.yml up -d'
                        } catch (e) {
                            echo "Deploy Frontend failed: ${e}"
                            error "Deploy Frontend failed"
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
                        } catch (e) {
                            echo "Build and Test Order Service failed: ${e}"
                            error "Build and Test Order Service failed"
                        }
                    }
                }
            }
        }

        stage('Deploy Order Service') {
            steps {
                dir('order-service') {
                    script {
                        echo 'Deploying order service using Docker Compose...'
                        try {
                            sh 'sudo docker-compose -f docker-compose.yml up -d'
                        } catch (e) {
                            echo "Deploy Order Service failed: ${e}"
                            error "Deploy Order Service failed"
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
                        } catch (e) {
                            echo "Build and Test Product Service failed: ${e}"
                            error "Build and Test Product Service failed"
                        }
                    }
                }
            }
        }

        stage('Deploy Product Service') {
            steps {
                dir('product-service') {
                    script {
                        echo 'Deploying product service using Docker Compose...'
                        try {
                            sh 'sudo docker-compose -f docker-compose.yml up -d'
                        } catch (e) {
                            echo "Deploy Product Service failed: ${e}"
                            error "Deploy Product Service failed"
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
                        } catch (e) {
                            echo "Build and Test User Service failed: ${e}"
                            error "Build and Test User Service failed"
                        }
                    }
                }
            }
        }

        stage('Deploy User Service') {
            steps {
                dir('user-service') {
                    script {
                        echo 'Deploying user service using Docker Compose...'
                        try {
                            sh 'sudo docker-compose -f docker-compose.yml up -d'
                        } catch (e) {
                            echo "Deploy User Service failed: ${e}"
                            error "Deploy User Service failed"
                        }
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
            script {
                echo 'Pipeline completed successfully!'
                emailext (
                    to: 'rishabhl@proteantech.in',
                    subject: "SUCCESS: ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                    body: """The build for job ${env.JOB_NAME} was successful.

                        - Build Number: ${env.BUILD_NUMBER}
                        - Build URL: ${env.BUILD_URL}

                        Please check the details at the above URL.
                    """
                )
            }
        }
        failure {
            script {
                echo 'Pipeline failed!'
                emailext (
                    to: 'rishabhl@proteantech.in',
                    subject: "FAILURE: ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                    body: """The build for job ${env.JOB_NAME} has failed.

                        - Build Number: ${env.BUILD_NUMBER}
                        - Build URL: ${env.BUILD_URL}

                        Please check the details at the above URL.
                    """
                )
            }
        }
    }
}
