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
		stage('nexus deploy'){
			steps{
			nexusArtifactUploader artifacts: [[artifactId: 'pets-app', classifier: '', file: 'target/pets-app.war', type: 'war']], 
					credentialsId: 'nexus3', 
					groupId: 'in.javahome', 
					nexusUrl: '172.31.6.101:8081', 
					nexusVersion: 'nexus3', 
					protocol: 'http', 
					repository: 'pets-app-snapshot', 
					version: '1.0-SNAPSHOT'
			}
		}
	}
}
