branchName = ''

branchName = env.BRANCH_NAME
echo('Andrew!!!!  branchName: ' + branchName)

//see: https://jenkins.io/doc/pipeline/steps/workflow-multibranch/#-properties-%20set%20job%20properties

def triggers = []
triggers << [
			$class: 'hudson.triggers.TimerTrigger',
			spec  : "00 05 * * *"
]
echo('Andrew!!!! triggers is: ' + triggers)
echo('Andrew!!! pipelineTriggers is: ' + pipelineTriggers(triggers) )  // is this a method call??

pipeline {
  agent any
  stages {
    stage('Andrew Start Initializeing') {
      steps {
        echo 'Hello Andrew Message'
        
        // https://wiki.jenkins.io/display/JENKINS/Pipeline+Maven+Plugin
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
