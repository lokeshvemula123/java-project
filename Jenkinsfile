pipeline {
   environment {
     git_url = "https://github.com/lokeshvemula123/jenkins-job.git"
     git_branch = "master"
   }

  agent any
 
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: 'dd06b1a1-9ae8-4d9d-8d14-58cd9c4189a7', branch: "${git_branch}", url: "${git_url}"
       
      }
     }
    
   
     stage('Docker Image Build') {     
        steps {
              sh 'sudo docker build -t myjava-image . '
               }
             }
        stage('Docker image push') {
           steps {
                 withCredentials([usernamePassword(credentialsId: 'd549a3d0-cefe-4da8-b105-f99f40e169d4', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 sh "sudo docker login -u ${env.Username} -p ${env.Password}"
                 sh "sudo docker image tag myjava-image lokeshvemula123/myjava-image:test"
                 sh "sudo docker image push lokeshvemula123/myjava-image:test" 
               } 
             }  
          }
      stage('Deploy app') {
         steps {
            sh 'kubectl apply -f app-deploy.yaml'
         }
      }
    }

  post {
    always {
      deleteDir() /* cleanup the workspace */
    }
  }
  }
