pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Rookiep/ci-cd-ja.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('myapp:latest')
                }
            }
        }
        stage('Push Docker Image') {
           steps {
              script {
            docker.image('myapp:latest').push('latest')
        }
    }
}
        stage('Deploy to Minikube') {
            steps {
                ansiblePlaybook credentialsId: 'ssh-credentials', playbook: 'ansible/deploy.yaml'
            }
        }
    }
}