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
                sh 'docker login --username=aliyun1489854349 registry.cn-hangzhou.aliyuncs.com --password=qing@aliyun2'
                sh 'docker tag devops:v1 registry.cn-hangzhou.aliyuncs.com/test-devops1/devops:v1'
                echo 'docker push registry.cn-hangzhou.aliyuncs.com/test-devops1/devops:2.0'
            }
        }
        stage("配置k8s环境"){
            steps {
                sh 'mkdir -p ~/.kube'
                sh 'cp conf1 ~/.kube/conf'
            }
        }
        stage('部署到Kubernetes') {
            steps {
             sh 'kubectl apply -f application/deployment.yaml'
            }
        }
    }
}