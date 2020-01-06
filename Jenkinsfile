pipeline {
  agent any

  tools {
    maven "apache-maven-3.6.3"
  }

  stages {
    stage('Build') {
      steps {
        git 'https://github.com/ajlanghorn/dvja.git'
        sh "mvn clean package"
      }
    }
    stage('Publish to S3') {
      steps {
        sh "aws s3 cp /var/lib/jenkins/workspace/dvja/target/dvja-1.0-SNAPSHOT.war s3://ako20-1578173747-buildartifacts-ja155umaqvzd/dvja-1.0-SNAPSHOT.war"
      }
    }
    stage('Check for vulnerabilities') {
      steps {
        script { startZap(host: "127.0.0.1", port: 9091, timeout: 500, zapHome: "/opt/zaproxy", "sessionPath": "session", allowedHosts:['github.com']) }
        sh "mvn verify -Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=9091 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=9091"
        script { archiveZap(failAllAlerts: 1, failHighAlerts: 0, failMediumAlerts: 0, failLowAlerts: 0, falsePositivesFilePath: "zapFalsePositives.json") }
        }
      }
    }
    stage('Tidy up') {
      steps {
        cleanWs()
      }
    }
  }
}
