pipeline {
  agent any
  tools { 
        maven 'Maven_3.5.2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=camtestbuggywebapp -Dsonar.organization=camtestbuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=6441affdf0663c222bae153e5afaea4ac8d737a8'
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
                 app =  docker.build("devsecopsimage")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://552295330352.dkr.ecr.ap-southeast-2.amazonaws.com', 'ecr:ap-southeast-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
