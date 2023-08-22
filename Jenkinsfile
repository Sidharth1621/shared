pipeline {
agent { label 'eds-slave' }
environment{ var1="null" }
stages { 
		stage('cleaning workspace'){
			steps{
			cleanWs()
			}
		}
		stage('Cloning from Git-repo'){
			steps{
				script{
					
					def GIT_CRED='git-credentials'
					def GIT_REPO_URL='git-url'
					checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: "${GIT_CRED}", url: "${GIT_REPO_URL}"]]])
					}
				}
		}
		stage('Maven Build') {
			steps {
				withMaven(maven: 'LGDOP Maven', mavenSettingsConfig: 'settings.xml') {
					script {
						sh """
							mvn clean install 
							"""
							}
							}
					}
		}
		stage('Upload Artifact to Nexus'){
			steps {
nexusArtifactUploader artifacts: [[artifactId: 'edsws', classifier: '', file: 'edsws-$BUILD_NUMBER.tar.gz', type: 'tar.gz']], credentialsId: 'jenkins-nexus-reader', groupId: 'LibertyGlobal/OSS/EDS/EDS_nb_apis', nexusUrl: 'devops.upc.biz/nexus', nexusVersion: 'nexus3', protocol: 'https', repository: 'releases', version: '$BUILD_NUMBER'
			}
		}
	}
}



