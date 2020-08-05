pipeline {
 
    environment {
        dockerregistry = 'https://registry.hub.docker.com'
        dockerhuburl = "novanovn/chitchat"
        githuburl = "novanovn/chitchat"
        dockerhubcrd = '94992ee7-67a1-4a48-8086-d9a1d2d1ddb3'
    }
 
    agent any
 
    tools {nodejs "nodejs1017"}
 
    stages {
 
        stage('Clone git repo') {
            steps {
                git 'https://github.com/' + githuburl
            }
        }
 
        stage('Install Node.js dependencies') {
            steps {
                sh 'npm install'
            }
        }
 
        stage('Test App') {
            steps {
                sh 'npm test'
            }
        }
 
        stage('Build image') {
          steps{
            script {
              dockerImage = docker.build(dockerhuburl + ":$BUILD_NUMBER")
            }
          }
        }
 
        stage('Test image') {
            steps {
                sh 'docker run -i ' + dockerhuburl + ':$BUILD_NUMBER npm test'
            }
        }
 
        stage('Deploy image') {
          steps{
            script {
              docker.withRegistry(dockerregistry, dockerhubcrd ) {
                dockerImage.push("${env.BUILD_NUMBER}")
                dockerImage.push("latest")
              }
            }
          }
        }
 
        stage('Remove image') {
          steps{
            sh "docker rmi $dockerhuburl:$BUILD_NUMBER"
          }
        }
    }
}
