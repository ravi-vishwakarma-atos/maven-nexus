pipeline{
	agent any
	
	environment{
		
		CURRENT_VERSION="1.0.${BUILD_NUMBER}"
	}
	
	stages{
	
		stage ("Checkout from GitHub") {
				steps{
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github-private-key', url: 'git@github.com:ravi-vishwakarma-atos/maven-nexus.git']]]) 
			}
		}
		
		stage("Clean workspace"){
			steps{
				sh "mvn clean"
			}
		}
		
		stage("Update version"){
			steps{
				sh "mvn versions:set versions:commit -DnewVersion=${CURRENT_VERSION}"
			}
		}
		
		stage("Upload to Nexus"){
		steps{
				sh "mvn deploy -Dmaven.test.skip=true"
			}
		}
	}
}
