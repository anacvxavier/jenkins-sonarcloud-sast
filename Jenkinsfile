pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
      stage('Checkout') {
            steps {	
                  git 'https://github.com/anacvxavier/jenkins-sonarcloud-sast.git'
			}
        } 
      stage('Build'){
            steps{
                  sh 'mvn install'
            }
      }
      stage('Test'){
            steps{
                  sh 'mvn test'
            }
      }

    stage('SAST Scan') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=acvx01 -Dsonar.organization=acvx01 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=2d707c9262e6919a87fa77738fd6f11a1ba4b6af'
			}
        } 
  }
}
