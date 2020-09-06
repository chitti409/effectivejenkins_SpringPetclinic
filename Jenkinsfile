pipeline {
    agent { label 'master' }
    stages {
        stage ('Checkout') {
          steps {
            git 'https://github.com/cbabu85/effectivejenkins_SpringPetclinic.git'
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
            sh 'scp target/*.jar jenkins@34.126.89.141:/opt/pet/'
            sh "ssh jenkins@34.126.89.141 'nohup java -jar /opt/pet/spring-petclinic-1.5.1.jar &'"
          }
        }
    }
}
