pipeline {
    agent any

    environment {
        // ACR相关信息
        REGISTRY_URL = 'registry.cn-hangzhou.aliyuncs.com/test-devops1'
        REGISTRY_REPO_NAME = 'devops'
        REGISTRY_USERNAME = credentials('your-registry-credential-id') // 替换为您的ACR凭据ID
        REGISTRY_PASSWORD = credentials('your-registry-secret') // 替换为您的ACR凭据密钥
        IMAGE_NAME = "${REGISTRY_REPO_NAME}/${REGISTRY_REPO_NAME}"

        // Kubernetes配置
        KUBECONFIG_PATH = '/var/jenkins/kubeconfig' // 或者省略此行如果Jenkins在集群内
    }

    stages {
        stage('拉取代码') {
            steps {
                git branch: 'main', url: 'https://github.com/chuum/devops.git'
            }
        }

        stage('构建Docker镜像') {
            steps {
                script {
                    def customImage = docker.build("${IMAGE_NAME}:latest")
                    customImage.push()
                }
            }
        }

        stage('部署到Kubernetes') {
            steps {
                script {
                    if (fileExists(KUBECONFIG_PATH)) {
                        withEnv(["KUBECONFIG=${UBECONFIG_PATH}"]) {
                            sh """
                                kubectl apply -f kubernetes/deployment.yaml
                                kubectl rollout status deployment/${DEPLOYMENT_NAME} --watch=true
                            """
                        }
                    } else {
                        error "kubeconfig file not found at ${UBECONFIG_PATH}. Please configure properly."
                    }
                }
            }
        }
    }

    }

    post {
        always {
            cleanWs() // 清理工作空间
            echo 'Pipeline completed'
        }
        failure {
            emaile extrecipients: 'your-email@example.com',
                subject: 'Jenkins Job Failure: ${JOB_NAME}#${BUILD_NUMBER}',
                body: "Check console output at ${env.BUILD_URL} for details."
        }
    }
}