pipeline {
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

    }
    
}
