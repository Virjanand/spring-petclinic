pipeline {
  agent { label 'slave01' }
  stages {
    stage ('Checkout') {
      steps {
        git 'https://github.com/Virjanand/spring-petclinic.git'
      }
    }
    stage('Build') {
      agent { docker 'maven:3.5-alpine' }
      steps {
        sh 'mvn clean package'
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }
    stage('Deploy') {
      steps {
        input 'Do you approve the deployment?'
        sh 'scp target/*.jar jenkins@192.168.33.10:/opt/pet/spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar'
        sh "ssh jenkins@192.168.33.10 'nohup java -jar /opt/pet/spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar &'"
      }
    }
  }
}
