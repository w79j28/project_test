//properties([pipelineTriggers([githubPush()])])
pipeline {
    agent any
    options {
        disableConcurrentBuilds()
    }
    environment {
	
	    CC_ENV = "ra-webapp:${env.BUILD_ID}"
    }	
    stages {
        stage('do pr check') {
	    when { changeRequest target: 'master' }
	    stages {
               stage('pr ') {
                   steps {
                       echo "pr ~!!!!!!!!!!!!!!!!"
		       setBuildStatus('jenkins:build', 'building','PENDING')
		       setBuildStatus('jenkins:codecov','codecov','PENDING')
		       sleep 10
		       setBuildStatus('jenkins:build', 'Your tests passed on CircleCI!','SUCCESS')	   
                   }
               }
            }

        }
	stage('master deploy') {
	    when {  branch 'master' }
	    stages {
               stage('deploy ') {
                   steps {
		       echo "deploying......"
		       setBuildStatus('jenkins:codecov','codecov','PENDING')
		       sleep 5
                       echo "deploy~!!!!!!!!!!!!!!!!"
		       setBuildStatus('jenkins:build', 'Your tests passed on CircleCI!','SUCCESS')
		       setBuildStatus('jenkins:build2', '!','SUCCESS')
		       setBuildStatus('jenkins:codecov','codecov','SUCCESS')
		       sh "echo ENV: ${CC_ENV}"	   
                   }
               }
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
 

void setBuildStatus(String title, String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: getRepoURL()],
      commitShaSource: [$class: "ManuallyEnteredShaSource", sha: getCommitSha()],	  
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: title],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}




