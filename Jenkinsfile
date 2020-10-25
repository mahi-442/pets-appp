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

		stage('Deploy-Tomcat'){

			steps{
				sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''/opt/tomcat8/bin/shutdown.sh
				/opt/tomcat8/bin/startup.sh''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'target', sourceFiles: 'target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
				
			}
		}	
	}

	post {
  		success {
    		// Send success message.
			mail bcc: '', body: '''Hi Team,
The build completed successfully.
Thanks,
DevOps Team.''', 
				subject: 'BUILD - SUCCESS', to: '9618714521m@gmail.com'
  		}
  		failure {
    		// Send failure message.
			mail bcc: '', body: '''Hi Team,
The build Failed.
Thanks,
DevOps Team.''', 
				subject: 'BUILD - FAILED', to: '9618714521m@gmail.com'
  		}
}
}

