pipeline {
  agent any
  tools { maven 'Maven_3_5_2' }

   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=acvx01 -Dsonar.organization=acvx01 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=2d707c9262e6919a87fa77738fd6f11a1ba4b6af'}
    }
   }

    stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'}
			}
   }
                
    }//daqui para baixo



} //pipeline closure  
