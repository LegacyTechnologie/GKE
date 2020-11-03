pipeline {
    agent any
    stages {
        stage('Set Project and Zone') {
          steps {
            withCredentials([[$class: 'FileBinding', credentialsId:"gcloud", variable: 'JSON_KEY']]) {
              sh 'gcloud auth activate-service-account --key-file $JSON_KEY'
              sh 'gcloud --version'
              // sh 'gcloud config set project myproject-ahsan-123'
              sh 'gcloud config set compute/zone us-central1-f'
            }
          }    
        }
        // stage('Build and push image with Container Builder') {
        //   steps {
        //     withCredentials([[$class: 'FileBinding', credentialsId:"gcloud", variable: 'JSON_KEY']]) {
        //       sh 'gcloud builds submit -t gcr.io/myproject-ahsan-123/helloworld-gke .'
        //     }
        //   }
        // }
        // stage('Create cluster') {
        //   steps {
        //     withCredentials([[$class: 'FileBinding', credentialsId:"gcloud", variable: 'JSON_KEY']]) {
        //       sh 'gcloud container clusters create helloworld-gke --num-nodes 1'
        //     }
        //   }
        // }
        // stage('Get cluster credentials') {
        //   steps {
        //     withCredentials([[$class: 'FileBinding', credentialsId:"gcloud", variable: 'JSON_KEY']]) {
        //       sh 'gcloud container clusters get-credentials helloworld-gke'
        //     }
        //   }
        // }
        // stage('Deployment') {
        //   steps {
        //     withEnv(["GOOGLE_CLOUD=/home/ahsan_sheraz_legacytechnologies_/google-cloud-sdk/bin"]){
        //       sh '$GOOGLE_CLOUD/kubectl apply -f $WORKSPACE/deployment.yaml'
        //       sh '$GOOGLE_CLOUD/kubectl apply -f $WORKSPACE/service.yaml'
        //       sh '$GOOGLE_CLOUD/kubectl get services'
        //     }
        //   }
        // }
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
        stage('Delete') {
          steps {
            sh 'gcloud container clusters delete helloworld-gke --quiet'
            sh 'gcloud container images delete gcr.io/myproject-ahsan-123/helloworld-gke --quiet'
          }
        }
    }
}
