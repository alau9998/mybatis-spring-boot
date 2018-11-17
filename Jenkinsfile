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

// https://qa.nuxeo.org/jenkins/pipeline-syntax/globals
// https://javadoc.jenkins-ci.org/index.html?hudson/model/Run.html
//andrewbuildtrigger = currentBuild.rawBuild?.getCause(hudson.triggers.TimerTrigger$TimerTriggerCause)?.getShortDescription() ?: "Unknown"
// andrewbuildtrigger = currentBuild.rawBuild?.getCauses()?.each( { echo "andrew build cause ${it}" } )
// above is same as below
// BELOW method is rejected, it says i am not administrator??
// below got an exception: Caused: java.io.NotSerializableException: hudson.model.Cause$UserIdCause
//see   https://stackoverflow.com/questions/37864542/jenkins-pipeline-notserializableexception-groovy-json-internal-lazymap
/*
I ran into this myself today and through some bruteforce I've figured out both how to resolve it and potentially why.

Probably best to start with the why:

Jenkins has a paradigm where all jobs can be interrupted, paused and resumable through server reboots. To achieve this the pipeline and its data must be fully serializable - IE it needs to be able to save the state of everything. Similarly, it needs to be able to serialize the state of global variables between nodes and sub-jobs in the build, which is what I think is happening for you and I and why it only occurs if you add that additional build step.

For whatever reason JSONObject's aren't serializable by default. I'm not a Java dev so I cannot say much more on the topic sadly. There are plenty of answers out there about how one may fix this properly though I do not know how applicable they are to Groovy and Jenkins. See this post for a little more info.

How you fix it:

If you know how, you can possibly make the JSONObject serializable somehow. Otherwise you can resolve it by ensuring no global variables are of that type.

Try unsetting your object var or wrapping it in a method so its scope isn't node global.
*/
@NonCPS
def andrewPrintTriggerCause() {
  //andrewbuildtrigger = 
  currentBuild.rawBuild?.getCauses()?.each( { it -> echo "andrew build cause ${it}" } )
}
//andrewPrintTriggerCause()

//echo('andrew get trigger description: ' + andrewbuildtrigger)
echo('Andrew build started!!!!!')

pipeline {
  agent any
  stages {
    stage('Andrew Start Initializeing') {
      steps {
        echo 'Hello Andrew Message'
	      
        andrewPrintTriggerCause()
        
        // https://wiki.jenkins.io/display/JENKINS/Pipeline+Maven+Plugin
	      // this is equivalent to the withMaven(...) {....} syntax
        withMaven({ sh "mvn -v" })
      }
    }
    stage('Andrew maven build') {
      //try to use docker to work around the jdk build bug (below
      // got error: https://stackoverflow.com/questions/47854463/got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket-at-uni
      // fixed: on my ubuntu box:   sudo usermod -a -G docker alau9998
      //no, just tried typing the failed command on my linux box while signin as alau9998:
      // still got permission error:  docker inspect -f . maven:3-alpine
      //   need to restart Jenkins ??
      // no, after i run this command   sudo usermod -a -G docker alau9998
      // then type:  groups             still don't see alau9998 added to docker group why??
      // no, type:   id alau9998             and it shows i am in that group
      // still not work
      // reboot Ubuntu box and it works!

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
        sh 'cat /etc/*-release'
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
