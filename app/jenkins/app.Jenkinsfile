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
        }
      }
    }  

    stage("Build Container") {
      steps {
        echo "Command to build container with app"
      }
    }

    stage("Run app") {
      steps {
        echo "Command to run application"
      }
    }

    stage("Test") {
      steps {
          echo "command to test application"
      }
    }

  }
   post { 
        success { 
          echo 'command to switch light on'
        }
        unstable { 
          echo 'command to switch light on'
        }
        failure { 
          echo 'command to switch light on'
        }
    }
}