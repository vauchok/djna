pipeline {
   environment {
     branch_name = 'master'
     artifact_home=/home/jenkins/workspace/${JOB_NAME}/app/build/outputs/apk
     nexus_rep='http://172.17.0.3:8081/repository/artifactory'
   }

   agent { label "docker-agent" }
   
   stages {
       stage("first") {
         steps{
           sh "echo ${branch_name}"
         }
       }
   }
}
