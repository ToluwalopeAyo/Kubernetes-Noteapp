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
    }
}