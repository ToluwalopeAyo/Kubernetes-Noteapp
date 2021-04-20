pipeline {
    agent any
    environment {
        PROJECT = "sca-tasks"
        APP_NAME = "noteapp"
        CLUSTER = "test"
        CLUSTER_ZONE = "us-east1-d"
        IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
        JENKINS_CRED = "${PROJECT}"
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage('Build and push image with Container Builder') {
            steps {
                container('gcloud') {
                    sh "PYTHONUNBUFFERED=1 gcloud builds submit -t ${IMAGE_TAG} ."
                }
            }
        }
    }
}   