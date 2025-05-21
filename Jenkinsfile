pipeline {
  agent any

  environment {
    AWS_REGION = 'ap-southeast-1'
    CLUSTER_NAME = 'eks-dev'
    HELM_HOME = '/opt/homebrew/bin/helm'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Update Kubeconfig') {
      steps {
        sh "aws eks update-kubeconfig --region ${AWS_REGION} --name ${CLUSTER_NAME}"
      }
    }

    stage('Helm Lint') {
      steps {
        dir('nginx-chart') {
          sh "${HELM_HOME} lint ."
        }
      }
    }

    stage('Helm Install/Upgrade') {
      steps {
        dir('nginx-chart') {
          sh "${HELM_HOME} upgrade --install nginx-app . -f values.yaml"
        }
      }
    }
  }

  post {
    success {
      echo "✅ Helm deployment successful."
    }
    failure {
      echo "❌ Helm deployment failed. Check logs."
    }
  }
}
