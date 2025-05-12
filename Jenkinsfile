pipeline {
    agent any
    
    environment {
        DOCKER_BACKEND_IMAGE = 'jobportal-backend'
        DOCKER_FRONTEND_IMAGE = 'jobportal-frontend'
        REGISTRY_URL = 'your-registry-url'  // Replace with your actual registry URL
        CLUSTER_NAME = 'your-cluster-name'  // Replace with your actual cluster name
        DOCKER_CREDENTIALS = credentials('docker-credentials')  // Jenkins credentials ID for Docker registry
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Backend Docker Image') {
            steps {
                dir('backend') {
                    script {
                        sh "docker build -t ${REGISTRY_URL}/${DOCKER_BACKEND_IMAGE}:${BUILD_NUMBER} ."
                    }
                }
            }
        }
        
        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    script {
                        sh "docker build -t ${REGISTRY_URL}/${DOCKER_FRONTEND_IMAGE}:${BUILD_NUMBER} ."
                    }
                }
            }
        }
        
        stage('Push Docker Images') {
            steps {
                script {
                    // Login to Docker registry
                    sh 'echo $DOCKER_CREDENTIALS_PSW | docker login $REGISTRY_URL -u $DOCKER_CREDENTIALS_USR --password-stdin'
                    
                    // Push both images
                    sh "docker push ${REGISTRY_URL}/${DOCKER_BACKEND_IMAGE}:${BUILD_NUMBER}"
                    sh "docker push ${REGISTRY_URL}/${DOCKER_FRONTEND_IMAGE}:${BUILD_NUMBER}"
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh '''
                        # Login to your cloud provider and configure kubectl
                        # Replace these with your actual cloud provider commands
                        # Example for IBM Cloud:
                        # ibmcloud login --apikey ${CLOUD_API_KEY} -r ${REGION} -g ${RESOURCE_GROUP}
                        # ibmcloud ks cluster config --cluster ${CLUSTER_NAME}
                        
                        # Update kubernetes deployment with new image tags
                        sed -i "s|image: .*backend.*|image: ${REGISTRY_URL}/${DOCKER_BACKEND_IMAGE}:${BUILD_NUMBER}|g" k8s/backend-deployment.yaml
                        sed -i "s|image: .*frontend.*|image: ${REGISTRY_URL}/${DOCKER_FRONTEND_IMAGE}:${BUILD_NUMBER}|g" k8s/frontend-deployment.yaml
                        
                        # Apply Kubernetes configurations
                        kubectl apply -f k8s/
                    '''
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
            // You can add notifications here (email, Slack, etc.)
        }
        failure {
            echo 'Pipeline failed! Check the logs for details.'
            // You can add failure notifications here
        }
        always {
            // Clean up Docker images to save space
            sh '''
                docker rmi ${REGISTRY_URL}/${DOCKER_BACKEND_IMAGE}:${BUILD_NUMBER} || true
                docker rmi ${REGISTRY_URL}/${DOCKER_FRONTEND_IMAGE}:${BUILD_NUMBER} || true
            '''
        }
    }
}
