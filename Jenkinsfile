pipeline {
   agent any 
   environment {
     version_tag= "${env.BUILD_ID}"
   }
   stages {
          stage('Copy html to web server root') {
               steps {
                 withCredentials([File(credentialsId: "websecret", Variable: 'keyfile')]) {
                  sh 'scp -i \${keyfile} index.html ubuntu@172.31.45.36:/home/ubuntu/html/'
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
