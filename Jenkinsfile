pipeline {
    agent any

    tools {
         maven 'maven3.9.6'
        }
        triggers {
        githubPush()
    }

    parameters {
  string defaultValue: '0.0.0', description: 'tag of the image', name: 'tag'
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
                sh '''
                docker build -t abc:latest .
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 905418329946.dkr.ecr.us-east-1.amazonaws.com
                docker tag abc:latest 905418329946.dkr.ecr.us-east-1.amazonaws.com/myecr:"${params.tag}"
                docker push 905418329946.dkr.ecr.us-east-1.amazonaws.com/myecr:"${params.tag}"
                '''
            }
        }
    
    }
    }
