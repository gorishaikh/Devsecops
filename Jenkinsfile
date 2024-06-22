pipeline {
  agent any
  tools { 
        maven 'Maven_3_8_4'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asgbuggywebapp4321_asgbuggywebapp -Dsonar.organization=asgbuggywebapp4321 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=1d9a396b5ff8118791b8570e4336531044562823'
			}
	}
	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'snyk_token', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }
	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "Docker_login", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://058264505919.dkr.ecr.ap-south-east-1.amazonaws.com/asg', 'ecr:ap-southeast-1:aws-credentials') {
                    app.push("latest")                                                                        
                    }
                }
            }
    	}
	   
   }
}