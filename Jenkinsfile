pipeline {
 
    agent any
 
    environment {
 
        GCR_CREDENTIALS_ID = 'P3' // The ID you provided in Jenkins credentials
 
        IMAGE_NAME = 'lbg'
 
        GCR_URL = 'gcr.io/lbg-mea-18/adr-project3'
 
        PROJECT_ID = 'lbg-mea-18'
 
        CLUSTER_NAME = 'demo-cluster-adr'
 
        LOCATION = 'europe-west2-c'
 
        CREDENTIALS_ID = 'SerAcc'
 
    }
 
    stages {
 
        stage('Build and Push to GCR') {
 
            steps {
 
                script {
 
                    // Authenticate with Google Cloud
 
                    withCredentials([file(credentialsId: GCR_CREDENTIALS_ID, variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
 
                        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
 
                    }
 
                    // Configure Docker to use gcloud as a credential helper
 
                    sh 'gcloud auth configure-docker --quiet'
 
                    // Build the Docker image
 
                    sh "docker build -t ${GCR_URL}/${IMAGE_NAME}:latest ."
 
                    // Push the Docker image to GCR
 
                    sh "docker push ${GCR_URL}/${IMAGE_NAME}:latest"
 
                }
 
            }
 
        }
 
        stage('Deploy to GKE') {
 
            steps {
 
                script {
 
                    // Deploy to GKE using Jenkins Kubernetes Engine Plugin
 
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'kubernetes/deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
 
                }
 
            }
 
        }
 
    }
 
}
