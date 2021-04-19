pipeline {

  environment {
    PROJECT = "sca-tasks"
    CLUSTER = "noteapp-cluster-prod"
    CLUSTER_ZONE = "us-west1"
    APP_NAME = "noteapp"
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