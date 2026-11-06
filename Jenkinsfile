pipeline {
  agent any

  environment {
    // Cambiar solo aquí si minikube se reinicia (ver: kubectl cluster-info)
    K8S_SERVER = 'https://127.0.0.1:49197'
    DEPLOYMENT = 'billing-app-back-deployment-jenkins'
  }

  stages {
    stage('Verify Tools') {
      steps {
        bat 'kubectl version --client'
      }
    }

    stage('Deploy billing App') {
      steps {
        withCredentials(bindings: [
          string(credentialsId: 'kubernetes-jenkis-server-account', variable: 'api_token')
        ]) {
          bat 'kubectl --token %api_token% --server %K8S_SERVER% --insecure-skip-tls-verify=true apply -f deployment-billing-app-back-jenkins.yaml'
          bat 'kubectl --token %api_token% --server %K8S_SERVER% --insecure-skip-tls-verify=true rollout restart deployment/%DEPLOYMENT%'
          bat 'kubectl --token %api_token% --server %K8S_SERVER% --insecure-skip-tls-verify=true rollout status deployment/%DEPLOYMENT% --timeout=180s'
        }
      }
    }
  }
}