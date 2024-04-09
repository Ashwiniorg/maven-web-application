pipeline {
    agent any

    tools {
         maven 'maven3.9.6'
        }
        
    stages {
        stage('git') {
            steps {
                git 'https://github.com/Ashwiniorg/maven-web-application.git'
            }
        }
        stage('maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('docker') {
            steps {
                sh 'docker build -t abc:latest .'
            }
        }
        stage('docker login') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 587437149137.dkr.ecr.us-east-1.amazonaws.com'
            }
        }
        stage('docker rename') {
            steps {
                sh 'docker tag abc:latest 587437149137.dkr.ecr.us-east-1.amazonaws.com/myecr:$BUILD_NUMBER'
            }
        }
        stage('docker push') {
            steps {
                sh 'docker push 587437149137.dkr.ecr.us-east-1.amazonaws.com/myecr:$BUILD_NUMBER'
            }
        }
    }
}