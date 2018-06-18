#!groovy
def server = Artifactory.server 'art'
def rtMaven = Artifactory.newMavenBuild()

// premerge
    // compile -> unittest -> clover -> ~sonar~ -> ~package~ -> ~artifactory~
// postmerge
    // compile -> unittest -> clover -> sonar -> package -> artifactory

pipeline{
    agent any

     tools {
        maven 'M3'
        jdk 'JDK8'
    }
    stages {
        stage('Build && SonarQube Analysis'){
            steps{
                sh 'mvn clean package sonar:sonar'
            }
        }
		stage('Publish'){
            steps{
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

}