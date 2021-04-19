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
                script {
                    def dockerHome = tool 'myDocker'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"
                }
            }
        }
    }
}