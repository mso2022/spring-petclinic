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
          bat 'C:\\Users\\Mitesh\\Desktop\\TODO\\5.KnowledgeHut\\Jenkins\\Module-10\\sonar-scanner-4.7.0.2747-windows\\bin\\sonar-scanner -Dsonar.projectVersion=1.0 -Dsonar.projectKey=spring-app -Dsonar.sources=src -Dsonar.java.binaries=.'
        }

        waitForQualityGate(abortPipeline: true, credentialsId: 'SonarToken')
      }
    }

    stage('Unit Tests') {
      agent {
        docker {
          image 'maven:latest'
          args '--network host -v $HOME/.m2:/root/.m2'
        }

      }
      steps {
        sh 'mvn test'
        junit '**/target/surefire-reports/TEST-*.xml'
        jacoco(changeBuildStatus: true, minimumLineCoverage: '10', runAlways: true)
      }
    }

  }
}