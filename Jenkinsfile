pipeline {
  agent any
  stages {
    stage('stage1') {
      steps {
        echo 'This is build $BUILD_NUMBER of demo $DEMO'
        sh 'echo "This is build $BUILD_NUMBER of demo $DEMO"'
        sh 'terraform version'
        sh 'printenv'
        sh 'terraform init'
        sh 'terraform plan'
        script { 
            if (env.CHANGE_ID) {
               pullRequest.addLabel('Build Failed')
            } else {
               echo 'This is not a PR'
            }
        }
      }
    }

  }
  environment {
    DEMO = '1'
  }
}
