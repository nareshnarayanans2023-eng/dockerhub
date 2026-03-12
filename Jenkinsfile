pipeline {
    agent any
    environment {
        // Remove the << >> brackets and use straight quotes
        DOCKER_IMAGE = "naresh876/html-demo"
    }
    stages {
        stage('Clone Code') {
            steps {
                // Fixed the URL and the curly quote at the end
                git branch: 'main', url: 'https://github.com/nareshnarayanans2023-eng/dockerhub.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }
        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    // Use -p for password but be aware it might show in logs
                    bat "docker login -u %USER% -p %PASS%"
                    bat "docker push %DOCKER_IMAGE%"
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kuberconfig', variable: 'KUBECONFIG')]) {
                    bat """
                    set KUBECONFIG=%KUBECONFIG%
                    kubectl apply -f deployment.yaml --validate=false
                    """
                }
            }
        }
    }
}
