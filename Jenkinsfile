pipeline{
	agent any
	tools {
		maven 'Maven3'
	}
	stages{	
		stage('Maven build/package'){
			steps{
				sh 'mvn clean package'
			}
		
	    }
	}
}
