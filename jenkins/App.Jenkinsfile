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
        sh "docker build -t demoapp ."
      }
    }

    stage("Run app") {
      steps {
        sh "docker run --name demoapp -p 9000:80 demoapp"
      }
    }

    stage("Test") {
      steps {
          sh "node test"
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