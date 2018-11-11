pipeline {
  agent any
  stages {
    stage('Andrew Start Initializeing') {
      steps {
        echo 'Hello Andrew Message'
      }
    }
    stage('Andrew maven build') {
      steps {
        echo 'Andrew calling mvn clean'
        bat 'C:\\downloads\\Jenkins\\andrew_mvn_clean_jenkins_pipeline_mybats_springboot.bat'
      }
    }
  }
}