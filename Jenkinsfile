pipeline {
  agent any
  tools { maven 'Maven_3_5_2' }

   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=acvx01 -Dsonar.organization=acvx01 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=2d707c9262e6919a87fa77738fd6f11a1ba4b6af'
		  }//fecha step
    }//fecha stage

    stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
        
					sh 'mvn snyk:test -fn'}
			}//fecha steps
   }//fecha stage

     stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("acvx")
                 }
               }
            }
    }//fecha stage

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://310877562977.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}//fecha stage	

	stage('Kubernetes Deployment of ASG Bugg Web Application') {
	   steps {
	      withKubeConfig([credentialsId: 'kubelogin']) {
		  sh('kubectl delete all --all -n devsecops')
		  sh ('kubectl apply -f deployment.yaml --namespace=devsecops')
		}
	      }
   	}//fecha stage

	stage ('wait_for_testing'){
	   steps {
		   sh 'pwd; sleep 180; echo "Application Has been deployed on K8S"'
	   	}
	   }//fecha stage
	   
	stage('RunDASTUsingZAP') {
          steps {
		    withKubeConfig([credentialsId: 'kubelogin']) {
				sh('zap.sh -cmd -quickurl http://$(kubectl get services/asgbuggy --namespace=devsecops -o json| jq -r ".status.loadBalancer.ingress[] | .hostname") -quickprogress -quickout ${WORKSPACE}/zap_report.html')
				archiveArtifacts artifacts: 'zap_report.html'
		    }
	     }
       }// fecha stage    
	   
}//fecha stages
	
}//fecha pipeline



 
