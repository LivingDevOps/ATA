#!groovy

pipeline {
  agent {
    label 'Host'
  }
  
  options {
    buildDiscarder(logRotator(numToKeepStr: '100', daysToKeepStr: '365'))
  }

  environment {
    BuildName = "ATA.Deploy.${BUILD_NUMBER}"
  }

  stages {
    stage('Set Build Informations') {
      steps {
        script {
          currentBuild.displayName = "App-Deploy.${BuildName}"
          def cause = currentBuild.rawBuild.getCause(hudson.model.Cause$RemoteCause)
          env.TriggerReason = cause.note
        }
      }
    }  

    stage("Info") {
      steps {
          sh 'printenv'
      }
    }


    stage("Build") {
      steps {
        sleep 3
      }
    }

    stage("Test") {
      steps {
        sleep 3
      }
    }

    stage("Deploy") {
      when {
        expression {
          return TriggerReason == "Off"
        }
      } 
      steps {
          error "Error"
      }
    }

  }
   post { 
        success { 
          sh 'curl http://adoplight.local:1880/jenkins/success'
        }
        unstable { 
          sh 'curl http://adoplight.local:1880/jenkins/failed'
        }
        failure { 
          sh 'curl http://adoplight.local:1880/jenkins/failed'
        }
    }
}