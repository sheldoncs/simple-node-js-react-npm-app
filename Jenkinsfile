pipeline {
  agent {
        any {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
  }
  tools {nodejs "nodejs"}
  stages {
    stage('Install Dependencies') {
      steps {
        git 'https://github.com/sheldoncs/simple-node-js-react-npm-app.git'
        sh 'npm install'
      }
    }
    stage('Build') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'dockercreds', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
             sh "docker login --username 'sheldoncs' --password 'Kentish@48'"
            //  sh "echo $PASS | docker login -u $USER -p $PASS" 
             sh 'docker build -t sheldoncs/sample-react-app .'
             sh 'docker push sheldoncs/sample-react-app'
            }
        }
    }
    stage('Deploy') {
        steps { 
          echo 'deploy'
        }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}