pipeline {
    agent any

    
    tools {
         maven 'maven3.9.6'
        }
        triggers {
        githubPush()
    }
    libraries {
  lib('mylibrary')
}

    parameters {
          string defaultValue: '0.0.4', description: 'tag of the image', name: 'tag'
         string defaultValue: '975421488220.dkr.ecr.us-east-1.amazonaws.com', description: 'url of registry', name: 'url'
         string defaultValue: 'myecr', description: 'repo for storing images', name: 'repo'
        }
        
    stages {
        stage('git') {
            steps {
                git 'https://github.com/Ashwiniorg/maven-web-application.git'
            }
        }
        stage('maven') {
            steps {
               build('Install')
            }
        }
        stage('Approval') {
            steps {
               timeout(5) {
    input ('proceed or abort?')
}
                    }
            
            }
        
        stage('docker') {
            steps {
                sh """
                docker build -t abc:latest .
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${params.url}
                docker tag abc:latest ${params.url}/${params.repo}:${params.tag}
                docker push ${params.url}/${params.repo}:${params.tag}
                """
            }
        }
    
    }
    }
