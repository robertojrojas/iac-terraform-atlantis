pipeline {
  agent any
  stages {
    stage('stage1') {
      steps {
        echo 'This is build $BUILD_NUMBER of demo $DEMO'
        sh 'echo "This is build $BUILD_NUMBER of demo $DEMO"'
        sh 'terraform version'
        sh 'printenv'
        sh 'terraform init -input=false'
        sh 'terraform plan -out=tf-plan -input=false -no-color | grep -E "(^.*[#~+-] .*|^[[:punct:]]|Plan|Terraform will)" > plan-output'
        script { 
           if (env.CHANGE_ID) {
              //withCredentials([string(credentialsId: 'gh-token', variable: 'GH_CREDENTIALS')]) {
              //withCredentials([usernamePassword(credentialsId: 'gh-up', usernameVariable: 'GHUSERNAME', passwordVariable: 'GHPASSWORD')]) {
                  //pullRequest.setCredentials("${GHUSERNAME}", "${GHPASSWORD}")
                  def plan = readFile("plan-output")
                  echo 'Plan from file'
                  echo "${plan}"
                  //pullRequest.comment('Terraform Plan running..')
                  input 'Ready to apply the Terraform plan?'
                  sh 'terraform apply -input=false tfplan' 
              // }
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
