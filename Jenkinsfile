pipeline {
   agent any 
   environment {
     version_tag= "${env.BUILD_ID}"
   }
   stages {
       withCredentials([sshUserPrivateKey(credentialsId: "privweb-key", keyFileVariable: 'keyfile')]) {
          stage('Copy html to web server root') {
               steps {
                  sh 'scp -i ${keyfile} index.html ubuntu@172.31.45.36:/home/ubuntu/html/'
               }
          }
       }
   }
   post {
      success {
         sh 'echo "SUCCESS"'
      }
      failure {
         sh 'echo "FAILURE"'
      }
   }
}
