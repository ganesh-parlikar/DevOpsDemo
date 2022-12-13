pipeline {
  agent {
    node {
      label 'java-app'
    }
  }
 tools { 
     maven 'Maven' 
 }		
  stages{
	stage(" Maven Build"){
		steps{
			sh 'mvn clean package'
		}
            }
        stage("Quality Check"){
		steps{
			withSonarQubeEnv('sonarscanner') {
				sh "mvn sonar:sonar"
			}
		}
      }
	stage("Upload to Nexus"){
		  steps{
			  nexusArtifactUploader artifacts: [
				  [
					  artifactId: 'DevOpsDemo', 
					  classifier: '', 
					  file: 'target/DevOpsDemo.war', 
					  type: 'war'
				  ]
			  ],
				  credentialsId: 'nexus', 
				  groupId: 'com.blazeclan', 
				  nexusUrl: '3.109.132.157:8081', 
				  nexusVersion: 'nexus3', 
				  protocol: 'http', 
				  repository: 'jenkins-assignment', 
				  version: '${BUILD_NUMBER}'
		  }
	  }
	  stage('Pull the Artifact from Nexus and Deploy on Production') {
		  agent{
			  node{
				  label 'java-app'
			  }
		  }
		  steps {
			  sh 'wget --user=admin --password=adminn http://3.109.132.157:8081/repository/jenkins-assignment/com/blazeclan/DevOpsDemo/14/DevOpsDemo-14.war'
			  deploy adapters: [
				  tomcat8(credentialsId: 'tomcat', 
					  path: '', 
					  url: 'http://13.235.242.99:8080/')
			  ], 
				  contextPath: '',
				  war: 'DevOpsDemo-14.war'
		  }
	  }
  }
}
