pipeline {
    agent any

    stages {
        stage('拉取代码') {
            steps {
                git branch: 'main', url: 'https://github.com/chuum/devops.git'
            }
        }

        stage('构建Docker镜像') {
            steps {
                sh 'docker build -t devops:v1 .'
            }
        }
        stage("推送镜像") {
            steps {
                echo 'push'
            }
        }
        stage('部署到Kubernetes') {
            steps {
             sh 'kubectl apply -f application/deployment.yaml'
            }
        }
    }
}