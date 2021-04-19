pipeline {
    agent any
    environment{
        PROJECT_ID = "sca-tasks"
        CLUSTER_NAME= "jenkins-cd"
        LOCATION = "us-east1-d"
        CREDENTIALS_ID = "sca-tasks"
        DOCKER_TAG = getDockerTag()
        IMAGE_URL_WITH_TAG = "tolulopeyo/k8s-noteapp:${DOCKER_TAG}"
    }
    stages {
        stage("checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                sh "docker build . -t ${IMAGE_URL_WITH_TAG}"
            }
        }
        stage("Push Image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        sh "docker push ${IMAGE_URL_WITH_TAG}"
                    }
                }
            }
        }
        stage("Deploy to GKE") {
            steps {
                sh "sed -i 's/k8s-noteapp:latest/k8s:${env.BUILD_ID}/g' ./kube/k8s-noteapp.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'kube/k8s-noteapp.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'kube/mongo.yaml', credentialsId: env.CREDENTIALS_ID ])
            }
        }
    }
}
def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}