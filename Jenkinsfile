#!/usr/bin/env groovy

pipeline {
  agent {
    label 'docker-agent'
  }
  environment {
    TARGET_URL='https://sbx-iqies.hcqis.org'
    CONTAINER_NAME='zap'
  }

  stages {
    stage('setup'){
      steps{
	// Start up the OWASP ZAP container
	//CONTAINER_ID=$(sh "docker run -u zap -p 2375:2375 -d owasp/zap2docker-weekly zap.sh -daemon -port 2375 -host 127.0.0.1 -config api.disablekey=true -config scanner.attackOnStart=true -config view.mode=attack -config connection.dnsTtlSuccessfulQueries=-1 -config api.addrs.addr.name=.* -config api.addrs.addr.regex=true")
	//CONTAINER_ID=$(sh "docker run -u zap -p 2375:2375 -d owasp/zap2docker-weekly zap.sh -daemon -port 2375 -host 127.0.0.1")
	sh "docker stop 52b661130cd7
	sh "docker run -u zap -p 2375:2375 -d owasp/zap2docker-bare zap.sh -daemon -port 2375 -host 127.0.0.1 --name zap" 
     	sh "docker ps"
      }
    }
    stage('test'){
      steps{
        sh "docker exec 52b661130cd7 zap-cli -p 2375 status -t 120 -v && docker exec 52b661130cd7 zap-cli -p 2375 -v open-url https://sbx-iqies.hcqis.org"
	sh "docker exec 52b661130cd7 zap-cli -p 2375 -v spider https://sbx-iqies.hcqis.org"
	sh "docker exec 52b661130cd7 zap-cli -p 2375 -v active-scan -r https://sbx-iqies.hcqis.org"
	sh "docker exec 52b661130cd7 zap-cli -p 2375 -v alerts"
        }
    }
  }
  post {
    always {
	// Bring the ZAP container down after the scan
	//sh "docker logs 52b661130cd7"  
	//sh "docker stop $CONTAINER_NAME"
	sh "docker ps"
    }
    success {
      echo 'Successful build!'
    }
    failure {
      echo 'Failed build!'
    }
  }
}
