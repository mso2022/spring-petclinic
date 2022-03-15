pipeline {
  agent any
  stages {
    stage('SCA') {
      agent {
        node {
          label 'windows'
        }

      }
      steps {
        withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarToken') {
          bat 'C:\\Users\\Mitesh\\Desktop\\TODO\\5.KnowledgeHut\\Jenkins\\Module-10\\sonar-scanner-4.7.0.2747-windows\\bin -Dsonar.projectVersion=1.0 -Dsonar.projectKey=spring-app -Dsonar.sources=src -Dsonar.java.binaries=.'
          waitForQualityGate(credentialsId: 'SonarToken', abortPipeline: true)
        }

      }
    }

  }
}