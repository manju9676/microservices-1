pipeline {
    agent any

    stages {
        stage('CQA'){
            steps {
                
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t manju9676/${env.BRANCH_NAME}:latest ."
                    }
                }
            }
        }
        stage('Image Scan'){
            steps {
                sh 'trivy image --severity HIGH,CRITICAL --exit-code 1 manju9676/emailservice:latest || true'
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push manju9676/${env.BRANCH_NAME}:latest "
                    }
                }
            }
        }
    }
}
