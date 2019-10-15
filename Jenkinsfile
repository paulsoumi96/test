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
    sh 'ssh -t remotehost "sudo -S /var/lib/jenkins/tools/hudson.plugins.gradle.GradleInstallation/gradle init"'
      sh 'ls -lat'
   sh 'ssh -t remotehost "sudo -S /var/lib/jenkins/tools/hudson.plugins.gradle.GradleInstallation/gradle build"'
   }
}
