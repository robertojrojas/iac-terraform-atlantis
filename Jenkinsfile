pipeline {
  agent any
  stages {
    stage('stage1') {
      steps {
        echo 'This is build $BUILD_NUMBER of demo $DEMO'
        sh 'echo "This is build $BUILD_NUMBER of demo $DEMO"'
        sh 'terraform version'
        script { 
           pullRequest.comment('This PR sTerraform plan....')
        }
      }
    }

  }
  environment {
    DEMO = '1'
  }
}
