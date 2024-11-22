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
	stage('RunSCAAnalysisUsingSnyk') {
            steps {        
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh 'mvn snyk:test -fn'
                }
            }
    	}
	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("jameswayne")
                 }
               }
            }
    }
	
	stage('Kubernetes Deployment of Capstone Web Application') {
	   steps {
	      withKubeConfig([credentialsId: 'kubelogin']) {
		  sh('kubectl delete all --all -n devsecops')
		  sh ('kubectl apply -f deployment.yaml --namespace=devsecops')
		}
	      }
   	}

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://302263080583.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:ap-south-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}	
    }
    
}
