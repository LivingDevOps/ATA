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
          sh "docker build -t demoapp ."
        }
      }
    }

    stage("Run app") {
      steps {
        sh "docker run --name testApp -d demoapp"
      }
    }

    stage("Test") {
      steps {
        dir("./app"){
          sh "docker exec testApp npm test"
        }
      }
    }

    stage("Deploy App") {
      steps {
        script {
          try {
            sh "docker rm -f prodApp"
          }
          catch(err) {
            echo "No container"
          }
          finally {
            sh "docker run --name prodApp -d -p 9000:80 demoapp"
          }
        }
      }
    }

  }

   post { 
       always { 
            script {
              try {
                sh "docker rm -f testApp"
              }
              catch(err) {
                echo "No container"
              }
            }
        }
        success { 
          script {
            currentBuild.description = "<a href=\"http://192.168.0.100:9000\" target=\"_blank\" >Productive App</a>"
          }
//          sh 'curl http://adoplight.local:1880/jenkins/success'
        }
//      unstable { 
//      }
//      failure { 
//      }
    }
}
