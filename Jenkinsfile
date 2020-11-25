pipeline {
    environment {
      PROJECT = "myproject-ahsan-123"
      APP_NAME = "aspnet-microservices-gke-direct"
      IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}"
    }
    agent any
    stages {
        stage('Set Project and Zone') {
          steps {
            withCredentials([[$class: 'FileBinding', credentialsId:"gcloud", variable: 'JSON_KEY']]) {
              sh 'gcloud auth activate-service-account --key-file $JSON_KEY'
              sh 'gcloud --version'
              // sh 'gcloud config set project myproject-ahsan-123'
              // sh 'gcloud config set compute/zone us-central1-f'
            }
          }    
        }
        stage('Build and push image with Container Builder') {
          steps {
            sh 'gcloud builds submit -t ${IMAGE_TAG} .'
          }
        }
        stage('Create cluster') {
          steps {
            sh 'gcloud container clusters create ${APP_NAME} --num-nodes 1 --machine-type=n1-standard-2'
          }
        }
        stage('Get cluster credentials') {
          steps {
            sh 'gcloud container clusters get-credentials ${APP_NAME}'
          }
        }
        stage('Deployment') {
          steps {
            withEnv(["GOOGLE_CLOUD=/home/ahsan_sheraz_legacytechnologies_/google-cloud-sdk/bin"]){
              sh '$GOOGLE_CLOUD/kubectl apply -f $WORKSPACE/deployment.yaml'
              sh '$GOOGLE_CLOUD/kubectl apply -f $WORKSPACE/service.yaml'
              sh 'sleep 60'
              sh '$GOOGLE_CLOUD/kubectl get services'
            }
          }
        }
        // stage('Get Services') {
        //   steps {
        //     withEnv(["GOOGLE_CLOUD=/home/ahsan_sheraz_legacytechnologies_/google-cloud-sdk/bin"]){
        //       sh '$GOOGLE_CLOUD/kubectl get nodes'
        //       sh '$GOOGLE_CLOUD/kubectl get deployments'
        //       sh '$GOOGLE_CLOUD/kubectl get pods'
        //       sh '$GOOGLE_CLOUD/kubectl get services'
        //     }
        //   }
        // }
        // stage('Delete') {
        //   steps {
        //     sh 'sleep 180'
        //     sh 'gcloud container clusters delete ${APP_NAME} --quiet'
        //     sh 'gcloud container images delete ${IMAGE_TAG} --quiet'
        //   }
        // }
    }
}
