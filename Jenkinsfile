node('docker-agent') {
  def branch = 'master'
  def url = 'http://172.17.0.2:8081/repository/artifactory/org/android'
  def file_path = "/home/jenkins/workspace/${JOB_NAME}/app/build/outputs/apk"
  stage('Pull from Git') {
    checkout scm: [$class: 'GitSCM', branches: [[name: "*/${branch}"]], userRemoteConfigs: [[url: 'https://github.com/vauchok/intro_android_demo.git/']]]
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
    sh "curl -v -u nexus:nexus --upload-file ${file_path}/app-release.apk ${url}/${BUILD_NUMBER}/android-${BUILD_NUMBER}.app-release.${BUILD_TIMESTAMP}"
  }

}
