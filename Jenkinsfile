pipeline {
    agent any
    
    stages {       

        stage('Install Node dependencies') {
            steps {
                bat "npm install"
            }
        }       
        
        stage('Run tests') {
            steps {
                bat "npm run test"
            }
        }
        
        stage('Build Docker Image and push it in Dockerhub') {
            steps {
                withCredentials([usernamePassword(credentialsId: '03f19296-bd09-456c-af79-b1c0002bb4d9', passwordVariable: 'token', usernameVariable: 'user')]) {
                    bat """docker build -t %user%/%app_name%:2.0.0 .
                        docker login --username %user% --password %token%
                        docker push %user%/%app_name%:2.0.0"""
                }
            }
        }
        
        stage('Deploy from Image') {
            steps {
                 withCredentials([usernamePassword(credentialsId: '03f19296-bd09-456c-af79-b1c0002bb4d9', passwordVariable: 'token', usernameVariable: 'user')]) {
                    bat """docker pull %user%/%app_name%:2.0.0
                        docker run -p 3030:3030 -d --name sr-container %user%/%app_name%:2.0.0"""
                }                
            }
        }
    }
}
