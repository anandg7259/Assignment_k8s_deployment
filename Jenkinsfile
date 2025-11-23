pipeline {
    agent any

    environment {
        KUBECONFIG = credentials('kubeconfig-cred')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/anandg7259/Assignment_k8s_deployment.git'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl --kubeconfig $KUBECONFIG apply -f deployment.yaml
                kubectl --kubeconfig $KUBECONFIG apply -f clusterIP.yaml
                """
            }
        }
    }

}
