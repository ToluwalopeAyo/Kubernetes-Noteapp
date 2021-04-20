pipeline {
    agent any
    environment{
        PROJECT_ID = "sca-tasks"
        CLUSTER_NAME= "noteapp-cluster-prod"
        LOCATION = "us-west1"
        CREDENTIALS_ID = "sca-tasks"
    
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Initilize") {
            steps {
                script{
                    def dockerHome = tool 'myDocker'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"
                }
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
                sh "sed -i 's/k8s-noteapp:latest/k8s-noteapp:${env.BUILD_ID}/g' k8s-note.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'mongo.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'k8s-note.yaml', credentialsId: env.CREDENTIALS_ID ])
            }
        }

    }
}    