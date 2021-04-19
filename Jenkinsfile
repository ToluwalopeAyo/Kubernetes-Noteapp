pipeline {
    agent any
    environment{
        PROJECT_ID = "sca-tasks"
        CLUSTER_NAME= "jenkins-cd"
        LOCATION = "us-east1-d"
        CREDENTIALS_ID = "sca-tasks"
    }
    stages {
        stage("checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                script {
                    noteapp = docker.build("tolulopeayo/k8s-noteapp:${env.BUILD_ID}")
                }
            }
        }
        stage("Push Image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        noteapp.push("latest")
                        noteapp.push("${env.BUILD_ID}")
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