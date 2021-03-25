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
		
		stage("Package"){
			steps{
				sh "mvn package"
			}
		}
		
		stage("Upload to Nexus"){
		steps{
			sh "mvn deploy:deploy-file \
  			-DgroupId=org.cloudifysource.examples \
  			-DartifactId=java-hello-world-webapp \
  			-Dversion=${CURRENT_VERSION} \
  			-DgeneratePom=true \
  			-Dpackaging=war \
  			-DrepositoryId=my-nexus \
 			 -Durl=http://35.232.63.148:8081/repository/maven-repo/ \
 			-Dfile=target/java-hello-world-webapp-${CURRENT_VERSION}.war"
			}
	
	
		}
		
		stage ("Install Tomcat"){
			steps{
				sh "ansible-playbook -i ansible-playbooks/tomcat-standalone/hosts ansible-playbooks/tomcat-standalone/site.yml"
			}
		}
		
		stage("Deploy to Tomcat")
		{
			steps{
			
   					sh "scp target/java-hello-world-webapp-${CURRENT_VERSION}.war tomcat-instance:/opt/apache-tomcat-7.0.61/webapps/"
					sh "ssh jenkins@tomcat-instance 'sudo /opt/apache-tomcat-7.0.61/bin/shutdown.sh; sudo /opt/apache-tomcat-7.0.61/bin/startup.sh'"
				
			}
			
		}
	}
}
