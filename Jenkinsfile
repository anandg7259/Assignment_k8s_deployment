pipeline {
    agent any  

    environment {
        KUBECONFIG = credentials('kubeconfig-cred')   ## our kubeconfig in Jenkins
        NODE_IP = "16.170.208.209"                    ## Node IP 
        NODE_PORT = "30001"                           ## NodePort from your service
    }

    stages {
        stage('Checkout & Deploy') {
            steps {
                git 'https://github.com/anandg7259/Assignment_k8s_deployment.git'
                sh """
                  kubectl --kubeconfig $KUBECONFIG apply -f deployment.yaml
                  kubectl --kubeconfig $KUBECONFIG apply -f nodeport.yaml
                  kubectl --kubeconfig $KUBECONFIG rollout status deployment/deployment-nginx
                """
            }
        }

        stage('Verify Access') {
            steps {
                script {
                    // Curl the app via NodePort
                    def result = sh(
                        script: "curl -s -o /dev/null -w \"%{http_code}\" http://${NODE_IP}:${NODE_PORT}",
                        returnStdout: true
                    ).trim()

                    if(result == "200") {
                        echo "SUCCESS: Application is reachable"
                    } else {
                        error("FAILURE: Application is NOT reachable, curl returned ${result}")
                    }
                }
            }
        }
    }
}
