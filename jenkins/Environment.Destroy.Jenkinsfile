#!groovy

pipeline {
  agent {
    label 'Host'
  }
  
  options {
    buildDiscarder(logRotator(numToKeepStr: '100', daysToKeepStr: '365'))
  }

  environment {
    BuildName = "ATA.${BUILD_NUMBER}"
  }

  stages {
    stage('Build Number') {
      steps {
        script {
          currentBuild.displayName = "Environment-Setup${BuildName}"
        }
      }
    }  

    stage("set up") {
      steps {
        sh 'chmod 600 ./key'
      }
    }

    stage("Stop Service") {
      steps {
        sh "ansible-playbook nodered/service.yml -vv state=absent"
        sh "ansible-playbook nodered/service.yml -vv --e \"state=absent\""


      }
    }

  }
}
