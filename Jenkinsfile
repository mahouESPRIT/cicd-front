pipeline {
  agent any

  tools {nodejs "NodeJSinstaller"}

  stages {
    stage('Checkout SCM') {
      steps{
      git branch : 'main', url:'https://github.com/mahouESPRIT/cicd-front.git'
       }
    }
    stage('Install node modules') {
      steps {
        sh 'npm install'
      }
    }
    stage('npm build') {
      steps {
        sh 'npm run build --prod'
      }
    }
     stage("Docker build"){
       steps {
        sh 'docker version'
        sh 'docker build -t esprit .'
        sh 'docker image list'
        sh 'docker tag esprit ayoubmahou/cicd-front:latest'
        
        withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
            sh 'docker login -u ayoubmahou -p $PASSWORD'
        }
       }
  }
    stage("Push Image to Docker Hub"){
      steps {
       sh 'docker push  ayoubmahou/cicd-front:latest'
    }
    }
     
        
}
   environment {
        EMAIL_TO = 'madmen1910@gmail.com'
    }
  post {
        failure {
            emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
        }
}
}
