#!groovy
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
    }
}