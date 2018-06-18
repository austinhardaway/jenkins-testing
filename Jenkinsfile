#!groovy
def server = Artifactory.server 'art'
def rtMaven = Artifactory.newMavenBuild()

pipeline{
    agent any

     tools {
        maven 'M3'
        jdk 'JDK8'
    }
    stages {
        stage('Build'){
            steps{
                sh 'mvn clean install'
            }
        }
		stage('Publish'){
			script {
				def uploadSpec = 	"""{
					"files": [
						{
						"pattern": "target/*.jar",
						"target": "example-repo-local/HelloWorld/"
						}
					]
				}"""
				server.upload(uploadSpec)
			}

		}
	}

}