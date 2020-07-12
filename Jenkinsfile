//Pass Status back to GitHub based on Trigger Type.
def setBuildStatus(String message, String state, String repo_url, String job_name, String commit_sha) {
    retry(3){
        step([
            $class: "GitHubCommitStatusSetter",
            reposSource: [$class: "ManuallyEnteredRepositorySource", url: repo_url],
            contextSource: [$class: "ManuallyEnteredCommitContextSource", context: job_name],
            errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: state]],
            commitShaSource: [$class: "ManuallyEnteredShaSource", sha: commit_sha],
            statusBackrefSource: [$class: "ManuallyEnteredBackrefSource", backref: "${BUILD_URL}display/redirect"],
            statusResultSource: [$class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
        ]);
    }
} 
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
            repo_url='https://github.com/robertojrojas/iac-terraform-atlantis'
            job_name="${JOB_BASE_NAME}"
            job_state='SUCCESS'
            commit_sha="${GITHUB_PR_HEAD_SHA}"
            message="terraform plan..."
            sendBuildStatus(message, job_state, repo_url, job_name, commit_sha)
        }
      }
    }

  }
  environment {
    DEMO = '1'
  }
}
