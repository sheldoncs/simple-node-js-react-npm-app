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
             sh 'docker build -t sheldoncs/sample-react-app:release-1.0 .'
             sh 'docker push sheldoncs/sample-react-app:release-1.0'
            }
        }
    }
    stage('Deploy') {
        steps { 
          echo 'echo Deploy to windows environment'
          node('windows-agent'){
                bat "\"C:\\valaxy\\test.bat\""
          }
        }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}