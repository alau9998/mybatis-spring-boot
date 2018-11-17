//node {   //this part maybe implied ...  https://jenkins.io/doc/book/pipeline/jenkinsfile/
//	checkout scm

//The checkout step will checkout code from source control;
//scm is a special variable which instructs the checkout step to clone the specific revision which triggered this Pipeline 


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
	//try to use docker to work around the jdk build bug (below
      agent { docker 'maven:3-alpine' }
      steps {
        echo 'Andrew calling mvn clean'
        //bat 'C:\\downloads\\Jenkins\\andrew_mvn_clean_jenkins_pipeline_mybats_springboot.bat'
	// the    mvn test     caused an error because of a jdk bug  https://stackoverflow.com/questions/53010200/maven-surefire-could-not-find-forkedbooter-class
	// 
	/*
	Picked up JAVA_TOOL_OPTIONS: -Dmaven.ext.class.path="/home/alau9998/.jenkins/workspace/_andrew-fourth-for-linux-jenkins@tmp/withMavenae673eca/pipeline-maven-spy.jar" -Dorg.jenkinsci.plugins.pipeline.maven.reportsFolder="/home/alau9998/.jenkins/workspace/_andrew-fourth-for-linux-jenkins@tmp/withMavenae673eca" 
Error: Could not find or load main class org.apache.maven.surefire.booter.ForkedBooter

there is a work around:

Set useSystemClassloader to false:

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <useSystemClassLoader>false</useSystemClassLoader>
    </configuration>
</plugin>
If you're not inheriting from a parent which has version defined for you (such as the spring boot starter) you'll need to define that as well.
	*/
	withMaven() { sh "mvn clean test" }
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
