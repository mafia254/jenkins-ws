pipeline {
  environment {
    registry = "mafia25/workshop-api"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  
  agent any
    
  tools {nodejs "node"}
    
  stages {
        
    stage('Cloning Git') {
      steps {
        git branch: 'master',
        credentialsId: 'github-arkha',
        url: 'https://github.com/cicd-staff/node-api.git'
      }
    }
        
    stage('Install dependencies') {
      steps {
        sh 'npm install'
      }
    }
     
    stage('Test') {
      steps {
         sh 'npm run test'
      }
    } 
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy') {
      steps{
        sh "docker-compose -f docker-compose.yml up -d --force-recreate"
        }
	  }
    stage('Push Image to Registry') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
  }
}
