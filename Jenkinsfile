pipeline{
  agent any
  environment {
    DOCKERHUB_CREDS = credentials('dockerhub')
  }
  stages {
    stage ('Clone Repo'){
      steps {
        checkout scm
        sh 'cat index.html'
      }
    }
    stage ('Build image'){
      steps {
        sh 'docker build -t bhairavisanskriti/sanskriti-portfolio:${BUILD_NUMBER} .' 
      }
    }
    stage ('Docker login') {
      steps {
        sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'
      }
    }
    stage ('Docker Push'){
      steps {
        sh 'docker push bhairavisanskriti/sanskriti-portfolio:${BUILD_NUMBER}'
      }
    }
    stage ('Update Manifest'){
      steps {
        build job: 'updateManifest', parameters: [string(name: 'BUILDNUMBER', value: "${BUILD_NUMBER}")]
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
