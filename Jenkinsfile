pipeline {
    agent { label "docker-agent" }

    environment {
      branch_name = 'master'
      artifact_home=/home/jenkins/workspace/${JOB_NAME}/app/build/outputs/apk
      nexus_rep='http://172.17.0.3:8081/repository/artifactory'
    }

    stages {
        stage('Test') {
            agent {
                docker-agent {
                    reuseNode true
                    image 'jenkinsci/jnlp-slave'
                }
            }
            steps {
              sh "echo ${branch_name}"
            }
        }
    }
}
