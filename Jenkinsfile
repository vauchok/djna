pipeline {
  agent {
    label 'docker-agent'
  }

  def branch_name = 'master'
  def artifact_home=/home/jenkins/workspace/${JOB_NAME}/app/build/outputs/apk
  def nexus_rep='http://172.17.0.3:8081/repository/artifactory'

  stage('Define Java/AndroidSDK paths') {
    sh "export JAVA_HOME=/opt/jdk1.8.0_161 && \
        export PATH=$JAVA_HOME/bin:$PATH && \
        export ANDROID_HOME=/opt/android-sdk-linux && \
        export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$PATH"
  }

  stage('Pull from Git') {
    checkout scm: [$class: 'GitSCM', branches: [[name: "*/${branch_name}"]], userRemoteConfigs: [[url: 'https://github.com/vauchok/intro_android_demo.git/']]]
  }
  
  stage('Creating the sign') {
    sh "mkdir -p /tmp/key/ && cd /tmp/key/ && \
        keytool -genkey -noprompt -alias mydomain -dname 'CN=info, OU=info, O=info, L=info, S=info, C=info' -keystore KeyStore.jks -storepass 1234567890 -keypass 1234567890 -keyalg RSA -keysize 2048 -validity 10000 && \
        keytool -importkeystore -srckeystore KeyStore.jks -srcstorepass 1234567890 -destkeystore KeyStore.jks -deststoretype pkcs12"
  }

  stage("Build"){
    sh "echo build artefact"
    sh "./gradlew clean build"
  }

  stage('Pushing artifact to nexus'){
    sh "curl -v -u nexus:nexus --upload-file ${artifact_home}/app-release.apk ${nexus_rep}/org/android/${BUILD_NUMBER}/android-${BUILD_NUMBER}.app-release.${BUILD_TIMESTAMP}"
  }

}
