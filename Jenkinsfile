pipeline {
agent any
    stages {
		stage('SCA') {
			agent
			{
				label 'terraform'
			}			
			steps {
				echo "Steps to execute SCA"
				withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarToken') {
					bat 'C:\\Users\\Mitesh\\Desktop\\TODO\\5.KnowledgeHut\\Jenkins\\Module-10\\sonar-scanner-4.7.0.2747-windows\\bin\\sonar-scanner -Dsonar.projectVersion=1.0 -Dsonar.projectKey=spring-app -Dsonar.sources=src -Dsonar.java.binaries=.'
				}
				waitForQualityGate(abortPipeline: true, credentialsId: 'SonarToken')
			}
		}
		stage('CI') {
			agent {
				docker {
					image 'maven:latest'
					args '--network host -v $HOME/.m2:/root/.m2'
				}
			}
			stages {

				stage('Unit Tests') {
					steps {
						sh 'mvn -version'
						sh 'java -version'
						sh 'mvn test'
						junit '**/target/surefire-reports/TEST-*.xml'
						jacoco buildOverBuild: true, changeBuildStatus: true, maximumLineCoverage: '10', runAlways: true
					}
				}			    
				stage('Build') {
					steps {
						sh 'mvn -version'
						sh 'java -version'
						sh 'mvn package -Dmaven.test.skip=true'
						archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
						stash(includes: 'target/*.jar', name: 'build')
					}
				}
			}
		}
		stage('Docker Image') {
			steps {
				unstash 'build'
				sh 'docker info'
				sh 'ls -l'
				sh 'docker build -f Dockerfile -t mitesh51/ms-petclinic:1.0 .'
				sh 'docker images'
	
				withDockerRegistry([ credentialsId: "dockerhub-cred", url: "" ]) {
					sh 'docker push mitesh51/ms-petclinic:1.0'
				}
			}
		}
		stage('Trivy Image Scanner') {
			agent {
				docker { 
					image 'aquasec/trivy:latest'
					args '--network host --entrypoint='
	
				}
			}  
			options { skipDefaultCheckout() }
			steps {
				sh 'trivy help'
				sh "trivy --cache-dir /tmp i 'mitesh51/ms-petclinic:1.0'"
			}
		}

    }
}
