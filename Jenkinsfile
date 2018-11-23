properties([pipelineTriggers([githubPush()])])
pipeline {
    agent any
    stages {
        stage('Start') {
            steps {
		    /*please specify repo, credentialsId, account and sha valuesSUCCESS*/
                //githubNotify description: 'my desc',  repo: getRepoURL(), credentialsId:'w79j28_github_user_password', account: 'w79j28', sha: getCommitSha(),  status: 'PENDING'
                setBuildStatus('build','PENDING')
		setBuildStatus('codecov','PENDING')
		sleep 20
		setBuildStatus('build','SUCCESS')
	    }
        }
    }
}	



def getRepoURL() {
  sh "git config --get remote.origin.url > .git/remote-url"
  return readFile(".git/remote-url").trim()
}
 
def getCommitSha() {
  sh "git rev-parse HEAD > .git/current-commit"
  return readFile(".git/current-commit").trim()
}
 
def updateGithubCommitStatus(String message, String state) {
  // workaround https://issues.jenkins-ci.org/browse/JENKINS-38674
  repoUrl = getRepoURL()
  commitSha = getCommitSha()
 
  step([
	$class: 'GitHubCommitStatusSetter',
	reposSource: [$class: "ManuallyEnteredRepositorySource", url: repoUrl],
	commitShaSource: [$class: "ManuallyEnteredShaSource", sha: commitSha],
	errorHandlers: [[$class: 'ShallowAnyErrorHandler']],
	statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ])
}
void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: getRepoURL()],
      commitShaSource: [$class: "ManuallyEnteredShaSource", sha: commitSha],	  
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: message],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}
