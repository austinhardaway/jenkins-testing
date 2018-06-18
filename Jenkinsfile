#!groovy
pipeline{
    agent any

     tools {
        maven 'M3'
        jdk 'JDK8'
    }
    stages {
        stage('Build'){
            sh 'mvn clean install'
        }
    }
}