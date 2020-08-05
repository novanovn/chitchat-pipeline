pipeline {

  environment {
    dockerregistry = 'https://registry.hub.docker.com'
    dockerhuburl = 'novanovn/chitchat'
    githuburl = 'novanovn/chitchat'
    dockerhubcrd = '94992ee7-67a1-4a48-8086-d9a1d2d1ddb3'
    dockerImage = ''
  }

  agent any

  tools {nodejs "nodejs1017"}

  stages {

    stage('Clone git repo') {
      steps {
         git 'https://github.com/' + githuburl
      }
    }
    stage('Code Quality Check via SonarQube') {
    steps {
       script {
       def scannerHome = tool 'sonar-scanner-4.3.0.2102-linux';
           withSonarQubeEnv("Sonarqube-Community") {
           sh "${tool("sonarqube")}/bin/sonar-scanner \
           -Dsonar.projectKey=chitchat-pipeline \
           -Dsonar.sources=. \
           -Dsonar.css.node=. \
           -Dsonar.host.url=http://10.8.3.93:9001 \
           -Dsonar.login=b61a3753ccc72e9ccf41fa126ebc990e6dcf63f2"

               }

            }

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


