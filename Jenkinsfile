pipeline{
	agent any
	tools {
		maven 'Maven3'
	}
	stages{
		stage('git Checkout'){
			steps{
				git credentialsId: 'github', url: 'https://github.com/mahi-442/pets-appp'
			}
		}	
		stage('Maven build/package'){
			steps{
				sh 'mvn clean package'
			}
		
	    }
	}
}
