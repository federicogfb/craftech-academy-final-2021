pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub2')
  }
  stages {
    stage('Login to Dockerhub') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Build Docker image') {
      steps {
        sh 'docker build -t federicogfb/practica-ci-jenkins .'
      }
    }
    stage('Push to Dockerhub') {
      steps {
        sh 'docker push federicogfb/practica-ci-jenkins'
      }
    }
    stage('Checkout code') {
            steps {
                checkout scm
            }
        }
 
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    def namespace = 'default' // Default namespace
                    def branch = env.BRANCH_NAME // Get the branch name from the environment
 
                    if (branch == 'main') {
                        namespace = 'production'
                    } else if (branch == 'dev') {
                        namespace = 'development'
                    } else {
                        namespace = 'feature' // or any other namespace for other branches
                    }
 
                    withCredentials([string(credentialsId: 'kubernetes', variable: 'KUBECONFIG')]) {
                        sh "kubectl --kubeconfig=$KUBECONFIG apply -f deployment/ --namespace=$namespace"
                    }
                }
            }
        }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}