pipeline {
    agent any
    tools {
        maven 'M3'
        jdk 'JDK8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
         stage('build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            } 
        }
        stage('Publish to Artifactory'){

            def server = Artifactory.newServer url: 'http://localhost:8081/artifactory/', username: 'admin', password: 'password'

            def uploadSpec =  """{
                    "files": [
                        {
                            "patern": "target/HelloWorld*.jar"
                        }
                    ]
                }
                """

            server.upload(uploadSpec)
        }
    }
}