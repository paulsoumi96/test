node {
   def app
   stage('Installations'){
      echo "gradle installation"
     tool name: 'gradle', type: 'gradle'
      echo "docker installation"
      tool name: 'docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
   }
   stage('Checkout') { 
      checkout scm
      workspace = pwd ()
       sh 'ls -lat' 
   }
   stage ('Build') {
      echo "initialization complete"
      sh './gradlew build'
   }
   stage ('Docker Image Build') {
       app = docker.build("paulsoumi96/test:${BUILD_NUMBER}")
   }
   stage ('Push Docker Image') {
       docker.withRegistry('https://registry.hub.docker.com','dockercredential') {
        		app.push("1-${BUILD_NUMBER}")
        		app.push("latest")
      	}
   }
   stage('Run Container') {
      sh "docker run -p 8082:8080 -d paulsoumi96/test"
   }
}
