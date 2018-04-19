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
    HueBridgeIp = "192.168.200.22"
    HueDimmerId = "6"
    HueLightId = "2"
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

    stage("Install needed packages") {
      steps {
        sh 'ansible-playbook nodered/install.yml -e="state=present"'
      }
    }

    stage("Startup service") {
      steps {
          withCredentials([usernameColonPassword(credentialsId: "adoplight", variable: "cred")]) {
            sh "ansible-playbook nodered/service.yml -vv --e \"credential=${cred} state=present bridgeIp=${HueBridgeIp} lightId=${HueLightId} dimmerId=${HueDimmerId}\""
          }
        }
      }

  }
}
