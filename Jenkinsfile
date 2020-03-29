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
				script{
					def pomfile = readMavenPom file: 'pom.xml'
			     	def version = pomfile.version
					def nexusRepo = version.endsWith("SNAPSHOT") ? "pets-app-snapshot" : "pets-app-release"
					nexusArtifactUploader artifacts: [[artifactId: 'pets-app', classifier: '', file: 'target/pets-app.war', type: 'war']], 
						credentialsId: 'nexus3', 
						groupId: 'in.javahome', 
						nexusUrl: '172.31.28.132:8081', 
						nexusVersion: 'nexus3', 
						protocol: 'http', 
						repository: 'pets-app-snapshot', 
						version: version
				}
			}
		}
	}
}
