pipeline {
    agent any

    environment {
        REGISTRY_URL = 'registry.cn-hangzhou.aliyuncs.com/test-devops1/devops'
        REGISTRY_USERNAME = credentials('aliyun1489854349')
        REGISTRY_PASSWORD = credentials('qing@aliyun2')
//         KUBECONFIG_PATH = '/path/to/your/kubeconfig' // 如果Jenkins运行在K8s集群内，可能不需要这行
    }

    stages {
        stage('拉取代码') {
            steps {
                git branch: 'main', url: 'https://github.com/chuum/devops.git'
                echo "1"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "2"
//                     def dockerImage = docker.build("test-devops1/devops:${env.BUILD_ID}")
                        sh "docker build -t test1:v1 ."
                    echo "3"
                }
            }
        }

        stage('Push Docker Image to ACR') {
            steps {
                script {
                    docker.withRegistry("${REGISTRY_URL}", "acr-credentials") {
                        def imageTag = "test-devops1/devops:${env.BUILD_ID}"
                        dockerImage.push(imageTag)
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                container('kubectl') { // 如果使用Kubernetes插件，确保有正确的container定义或直接使用sh命令
                    sh 'kubectl apply -f application/deployment.yaml '
                    // 确保k8s-deployment.yaml中引用的镜像标签与推送的一致，或者使用kustomize、helm等进行动态替换
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed :('
            emailext body: 'The job has failed. Check console output at ${BUILD_URL} for more details.', subject: 'Job Failed', to: 'your-email@example.com'
        }
    }
}

