#!/usr/bin/env groovy

pipeline {
  agent {
    label 'docker-agent'
  }
  stages {
    stage('setup'){
      /*    
      agent {
        docker 'owasp/zap2docker-bare'
      }
      */
      steps{
	sh "docker run --name zap -u zap -p 8090:8090 -i owasp/zap2docker-bare zap.sh -daemon -host 0.0.0.0 -port 8090"
	sh 'docker ps'
        // sh "zap.sh -daemon -port 8080 -host 0.0.0.0 -config api.disablekey=true"
        // sh "zap-cli quick-scan --spider -r http://ventera.com"
      }
    }
  }
  post {
    always {
      sh 'docker rm --force zap_container'
      sh 'docker ps'
    }
    success {
      echo 'alert successful build'
    }
    failure {
      echo 'alert failed build'
    }
  }
}
