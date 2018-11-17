branchName = ''

branchName = env.BRANCH_NAME
echo('Andrew!!!!  branchName: ' + branchName)

//see: https://jenkins.io/doc/pipeline/steps/workflow-multibranch/#-properties-%20set%20job%20properties

def triggers = []
triggers << [
			$class: 'hudson.triggers.TimerTrigger',
			spec  : "00 05 * * *"
			//this really works!!!!
			//spec: "*/3 * * * *" //every 3 minute ??
]
echo('Andrew!!!! triggers is: ' + triggers)
andrewTrigger = pipelineTriggers(triggers)
echo('Andrew!!! pipelineTriggers is: ' + andrewTrigger )  // is this a method call??

properties([andrewTrigger])
//properties([zzz])

pipeline {
  agent any
  stages {
    stage('Andrew Start Initializeing') {
      steps {
        echo 'Hello Andrew Message'
        
        // https://wiki.jenkins.io/display/JENKINS/Pipeline+Maven+Plugin
	      // this is equivalent to the withMaven(...) {....} syntax
        withMaven({ sh "mvn -v" })
      }
    }
    stage('Andrew maven build') {
      steps {
        echo 'Andrew calling mvn clean'
        //bat 'C:\\downloads\\Jenkins\\andrew_mvn_clean_jenkins_pipeline_mybats_springboot.bat'
      }
    }
    stage('Andrew Last stage') {
      steps {
        echo 'Andrew Last Stage'
        // bat 'C:\\downloads\\Jenkins\\andrew_mvn_build_install_jenkins_pipeline_mybats_springboot.bat'
      }
    }
  }
}
