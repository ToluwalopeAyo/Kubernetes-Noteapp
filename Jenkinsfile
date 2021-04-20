pipeline {
    agent any
    environment{
        PROJECT_ID = "sca-tasks"
        CLUSTER_NAME= "jenkins-cd"
        LOCATION = "us-east1-d"
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

    }
}    