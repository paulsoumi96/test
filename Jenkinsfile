node {
   stage('install gradle'){
   tool name: 'gradle', type: 'gradle'
   }
   stage('Checkout') { 
      checkout scm
      workspace = pwd ()
       sh 'ls -lat'
   }
   
stage ('Build') {
      // sh "'${mvn1}/bin/mvn' -Dmaven.test.failure.ignore clean package"
    sh 'sudo -S /var/lib/jenkins/tools/hudson.plugins.gradle.GradleInstallation/gradle init'
      sh 'ls -lat'
   sh 'sudo -S /var/lib/jenkins/tools/hudson.plugins.gradle.GradleInstallation/gradle build'
   }
}
