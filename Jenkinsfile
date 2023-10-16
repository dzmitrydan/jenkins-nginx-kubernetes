pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/dzmitrydan/jenkins-nginx-kubernetes.git'
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f k8s-manifests.yaml'
            }
        }
        stage('Test Deploy') {
            steps {
                sh 'kubectl get pod -l app=nginx-label'
                sh 'kubectl get service nginx-service'

            }
        }
        stage('Test Service') {
            steps {
                sh 'kubectl get service nginx-service -o jsonpath="{.status.loadBalancer.ingress[0].hostname}" | grep -q localhost && echo "Hostname localhost used by service" || exit 1'
                sh 'kubectl get service nginx-service -o jsonpath="{.spec.ports[0].port}" | grep -q 8083 && echo "Port 8083 used by service" || exit 1'
            }
        }
    }
}