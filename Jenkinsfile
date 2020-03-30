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
						repository: nexusRepo, 
						version: version
				}
			}
		}

		stage('Maven build/package'){

			steps{
				sshagent(['tomcat-devv']) {
					//to copy war file to tomcat webapps
					sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.28.41:/opt/tomcat8/webapps/pets-app"
					//start and stop tomcat
					sh "ssh ec2-user@172.31.28.41/opt/tomcat8/bin/shutdown.sh"
					sh "ssh ec2-user@172.31.28.41/opt/tomcat8/bin/startup.sh"
 				}
				
			}
		}	
	}
}
