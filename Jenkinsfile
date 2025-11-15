pipeline {

    agent {
        docker {
            image 'docker:24.0' 
            args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Rookiep/ci-cd-ja.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker tag myapp:latest parthomazumdar/myapp:latest'
                sh 'docker push parthomazumdar/myapp:latest'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                ansiblePlaybook(
                    credentialsId: 'ssh-credentials',
                    playbook: 'ansible/deploy.yaml',
                    extraVars: [
                        image_tag: "latest"
                    ]
                )
            }
        }
    }
}
