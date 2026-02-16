pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {
        stage('Deploy To Kubernetes') {
            steps {
                sh '''
                    echo "Checking EKS connectivity..."
                    kubectl get nodes

                    echo "Creating namespace if not exists"
                    kubectl create ns webapps --dry-run=client -o yaml | kubectl apply -f -

                    echo "Deploying manifests"
                    kubectl apply -f deployment-service.yml -n webapps
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    kubectl get pods -n webapps
                    kubectl get svc -n webapps
                '''
            }
        }
    }
}
