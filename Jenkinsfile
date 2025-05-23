pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "yeswanthteja/my-ecommerce-backend2"
        DOCKER_TAG = "${BUILD_NUMBER}"
        // REGISTRY = "yeswanthteja/my-ecommerce"
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main' , url: 'https://github.com/Yeswanthteja1010/ecommerce_april_2_practice.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'Docker_hub_cred', url: '']) {
                        // sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${REGISTRY}:${DOCKER_TAG}"
                        sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    }
                }
            }
        }
        
        stage('Deploy to Local Docker') {
            steps {
                script {
                    sh "docker stop ecommerce || true"
                    sh "docker rm ecommerce || true"
                    sh "docker run -d -p 5000:5000 --name ecommerce2 ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }
    
    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
