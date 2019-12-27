pipeline {
  agent any

  tools {
    maven "M3"
  }

  stages {
    stage('Build') {
      git 'https://github.com/ajlanghorn/dvja.git'
      sh "mvn clean package"
    }
  }
}
