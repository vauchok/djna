node('docker-agent') {

  def branch = 'master'
  def url = 'http://172.17.0.2:8081/repository/artifactory/org/android'
  def path_to_artifact = "/home/jenkins/workspace/${JOB_NAME}/app/build/outputs/apk"
  def storepass =
  def keypass =
  def alias = 

  stage('Pull from Git') {
    checkout scm: [$class: 'GitSCM', branches: [[name: "*/${branch}"]], userRemoteConfigs: [[url: 'https://github.com/vauchok/intro_android_demo.git/']]]
  }
  
  stage('Creating the signature') {
    sh "mkdir -p /tmp/key/ && cd /tmp/key/ && \
        keytool -genkey -noprompt -alias ${alias} -dname 'CN=info, OU=info, O=info, L=info, S=info, C=info' -keystore KeyStore.jks -storepass ${storepass} -keypass ${keypass} -keyalg RSA -keysize 2048 -validity 10000 && \
        keytool -importkeystore -srckeystore KeyStore.jks -srcstorepass ${storepass} -destkeystore KeyStore.jks -deststoretype pkcs12"
  }

  stage("Building app"){
    sh "./gradlew clean build"
  }

  stage('Pushing artifact to nexus'){
    sh "curl -v -u nexus:nexus --upload-file ${path_to_artifact}/app-release.apk ${url}/${BUILD_NUMBER}/android-${BUILD_NUMBER}.app-release.${BUILD_TIMESTAMP}"
  }
}
