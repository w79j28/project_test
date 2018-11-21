pipeline {
    agent any
	
    
	
    stages {
        stage('Start') {
            steps {
		    /*please specify repo, credentialsId, account and sha valuesSUCCESS*/
                githubNotify description: 'my desc',  repo: getRepoURL(), credentialsId:'w79j28_token', account: 'w79j28', sha: getCommitSha(),  status: 'PENDING'
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
 
def updateGithubCommitStatus(build) {
  // workaround https://issues.jenkins-ci.org/browse/JENKINS-38674
  repoUrl = getRepoURL()
  commitSha = getCommitSha()
 
  step([
	$class: 'GitHubCommitStatusSetter',
	reposSource: [$class: "ManuallyEnteredRepositorySource", url: repoUrl],
	commitShaSource: [$class: "ManuallyEnteredShaSource", sha: commitSha],
	errorHandlers: [[$class: 'ShallowAnyErrorHandler']],
	statusResultSource: [
	  $class: 'ConditionalStatusResultSource',
	  results: [
		[$class: 'BetterThanOrEqualBuildResult', result: 'SUCCESS', state: 'SUCCESS', message: build.description],
		[$class: 'BetterThanOrEqualBuildResult', result: 'FAILURE', state: 'FAILURE', message: build.description],
		[$class: 'AnyBuildResult', state: 'FAILURE', message: 'Loophole']
	  ]
	]
  ])
}
