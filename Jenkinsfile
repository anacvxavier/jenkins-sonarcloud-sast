pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=acvx01 -Dsonar.organization=acvx01 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=2d707c9262e6919a87fa77738fd6f11a1ba4b6af'
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
/*
   stage('Build') { 
            steps { 
              withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{ 
                  app =  docker.build("acvx") }
               }
            }
    }
    */
/*
	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://310877562977.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
                    app.push("latest")
                    }
                }
              } 
    }//daqui para baixo
*/



} //pipeline closure  
