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
          if(cause != null){
            env.TriggerReason = cause.note
          }
        }
      }
    }  

    stage("Build Container") {
      steps {
        dir("./app"){
          echo "command to build app"
        }
      }
    }

    stage("Run app") {
      steps {
          echo "command to run app"
      }
    }

    stage("Test") {
      steps {
        dir("./app"){
          echo "command to test app"
        }
      }
    }

    stage("Deploy App") {
      steps {
        script {
          try {
            echo "command to deploy app"
          }
          catch(err) {
            echo "command to deploy app"
          }
          finally {
            echo "command to deploy app"
          }
        }
      }
    }

  }

   post { 
       always { 
            script {
              try {
                echo "command to cleanup"
              }
              catch(err) {
                echo "command to cleanup"
              }
            }
        }
        success { 
          echo "command to enable lights"
        }
        unstable { 
          echo "command to enable lights"
        }
        failure { 
          echo "command to enable lights"
        }
    }
}