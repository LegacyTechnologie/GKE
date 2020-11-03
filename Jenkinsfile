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
        stage('Deployment') {
          steps {
            withCredentials([[$class: 'FileBinding', credentialsId:"gcloud", variable: 'JSON_KEY']]) {
              // sh 'gcloud container clusters get-credentials helloworld-gke'
              // sh("sed -i.bak 's#gcr.io/cloud-solutions-images/gceme:1.0.0#${IMAGE_TAG}#' ./k8s/production/*.yaml")
              step([$class: 'KubernetesEngineBuilder', projectId: "myproject-ahsan-123", clusterName: "helloworld-gke", zone: "us-central1-f", manifestPattern: '$WORKSPACE/', credentialsId: "gcloud", verifyDeployments: true])
              // sh 'kubectl apply -f deployment.yaml'
              // sh 'kubectl apply -f service.yaml'
              // sh 'kubectl get services'
            }
          }
        }
    }
}
