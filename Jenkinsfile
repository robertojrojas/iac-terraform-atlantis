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
              withCredentials([file(credentialsId: 'gh-creds', variable: 'GH_CREDENTIALS')]) {
                  pullRequest.setCredentials('', "${GH_CREDENTIALS}")
                  pullRequest.comment('Terraform Plan running..')
               }
           } else {
               echo 'Executing outside of a PR'
           }
        }
      }
    }

  }
  environment {
    DEMO = '1'
  }
}
