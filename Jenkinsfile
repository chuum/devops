
pipeline {
    agent any

    environment {
        REGISTRY_URL = 'registry.cn-hangzhou.aliyuncs.com/test-devops1/devops'
        REGISTRY_USERNAME = credentials('aliyun1489854349')
        REGISTRY_PASSWORD = credentials('qing@aliyun2')
    }

    stages {
        stage('拉取代码kmklmkl') {
            steps {
                git branch: 'main', url: 'https://github.com/chuum/devops.git'
            }
        }

        stage('Build Docker Imagehjghj') {
            steps {
                script {
                    sh "docker build -t test-devops:v1 ."
                }
            }
        }

        stage('Push Docker Image to ACR') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'acr-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        docker.withRegistry("${REGISTRY_URL}", "${USERNAME}", "${PASSWORD}") {
                            sh "docker tag test-devops:v1 ${REGISTRY_URL}:v1"
                            sh "docker push ${REGISTRY_URL}:v1"
                        }
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                container('kubectl') {
                    sh 'kubectl apply -f application/deployment.yaml'
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
//             emailext body: 'The job has failed. Check console output at ${BUILD_URL} for more details.', subject: 'Job Failed', to: 'your-email@example.com'
        }
    }
}
