pipeline {
    agent
    {
        label 'terraform'
    }
    stages {
        
        stage('SCA') {
		
			steps {
				echo "Steps to execute SCA"
				withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarToken') {
					bat 'C:\\Users\\Mitesh\\Desktop\\TODO\\5.KnowledgeHut\\Jenkins\\Module-10\\sonar-scanner-4.7.0.2747-windows\\bin\\sonar-scanner -Dsonar.projectVersion=1.0 -Dsonar.projectKey=spring-app -Dsonar.sources=src -Dsonar.java.binaries=.'
				}
				waitForQualityGate(abortPipeline: true, credentialsId: 'SonarToken')
			}
        }

    }
    
}
