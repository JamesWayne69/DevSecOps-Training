pipeline {
  agent any
  tools { 
        maven 'Maven_3_2_5'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=JamesWayne69_DevSecOps-Training -Dsonar.organization=waynecorp -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=1b5e5ec9418f7d33cf112277357bcb17bb88f666'
			}
		}
    }
	
	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

}
