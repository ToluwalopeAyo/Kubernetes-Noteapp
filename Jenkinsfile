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
        stage("Build image") {
            steps {
                script {
                    noteapp = docker.build("tolulopeayo/k8s-noteapp:${env.BUILD_ID}")
                }
            }
        }
    }
}