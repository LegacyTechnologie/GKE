pipeline {
    agent {
    kubernetes {
      label 'sample-app'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: cd-jenkins
  containers:
  - name: golang
    image: golang:1.10
    command:
    - cat
    tty: true
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command:
    - cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
"""
}
    }
    stages {
        stage('Set Project & Zone') {
            steps {
                sh 'gcloud config set project myproject-ahsan-123'
                sh 'gcloud config set compute/zone us-central1-f'
            }    
        }
        stage('Build and push image with Container Builder') {
          steps {
            container('gcloud') {
              sh "PYTHONUNBUFFERED=1 gcloud builds submit -t gcr.io/myproject-ahsan-123/helloworld-gke ."
            }
          }
        }
        stage('Create cluster') {
          steps {
            container('gcloud') {
              sh "PYTHONUNBUFFERED=1 gcloud container clusters create helloworld-gke --num-nodes 1"
            }
          }
        }
        stage('Get cluster credentials') {
          steps {
            container('gcloud') {
              sh "PYTHONUNBUFFERED=1 gcloud container clusters get-credentials helloworld-gke"
            }
          }
        }
        stage('Deployment') {
              steps {
                container('kubectl') {
                  // Create namespace if it doesn't exist
                  sh("kubectl apply -f $WORKSPACE/deployment.yaml")
                  // Don't use public load balancing for development branches
                  sh("kubectl apply -f $WORKSPACE/service.yaml")
                  sh("kubectl get services")
                }
            }
        }
    }
}
