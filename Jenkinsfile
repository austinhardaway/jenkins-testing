pipeline {
    agent any
    tools {
        maven 'M3'
        jdk 'JDK8'
    }

    node {
        def server
        def buildInfo
        def rtMaven

        stage ('Initialize') {
            server = Artifactory.newServer url: 'http://localhost:8081/artifactory/', username: 'admin', password: 'password'
            rtMaven = Artifactory.newMavenBuild()
            rtMaven.tool = M3
            rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server 
            rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
            rtMaven.deployer.deployArtifacts = false // Disable artifacts deployment during Maven run
            buildInfo = Artifactory.newBuildInfo()
        }
         stage('Test') {
            rtMaven.run pom: './pom.xml', goals: 'clean test'
        }
        stage('Install') {
            rtMaven.run pom: './pom.xml', goals: 'install', buildInfo: buildInfo

        }
        stage('Deploy') { 
            rtMaven.deployer.deployArtifacts buildInfo
        }
        stage('Publish to Artifactory'){
           server.publishBuildInfo buildInfo
        }
    }
}