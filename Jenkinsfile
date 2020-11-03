pipeline {
    agent any
    stages {
        stage('Set Project and Zone') {
            steps {
              withCredentials([[$class: 'FileBinding', credentialsId:"gcloud", variable: 'JSON_KEY']]) {
                // sh ('gcloud auth activate-service-account --key-file $GC_KEY')
                sh 'gcloud auth activate-service-account --key-file $JSON_KEY'
                // sh 'gcloud --version'
                // sh 'gcloud config set project myproject-ahsan-123'
                // sh 'gcloud config set compute/zone us-central1-f'
              }
            }    
        }
        // stage('Build and push image with Container Builder') {
        //   steps {
        //     sh 'gcloud builds submit -t gcr.io/myproject-ahsan-123/helloworld-gke .'
        //   }
        // }
        // stage('Create cluster') {
        //   steps {
        //     sh 'gcloud container clusters create helloworld-gke --num-nodes 1'
        //   }
        // }
        // stage('Get cluster credentials') {
        //   steps {
        //     sh 'PYTHONUNBUFFERED=1 gcloud container clusters get-credentials helloworld-gke'
        //   }
        // }
        // stage('Deployment') {
        //   steps {
        //     sh 'kubectl apply -f $WORKSPACE\\deployment.yaml'
        //     sh 'kubectl apply -f $WORKSPACE\\service.yaml'
        //     sh 'kubectl get services'
        //   }
        // }
    }
}
