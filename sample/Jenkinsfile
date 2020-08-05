pipeline {
    agent any
 
    tools {nodejs "nodejs1017"}
 
    stages {
 
        stage('Cloning Git') {
            steps {
                git 'https://github.com/novanovn/chitchat'
            }
        }
 
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
 
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
}
