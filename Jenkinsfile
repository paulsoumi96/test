node {
   def app
   stage('Installations'){
      echo "gradle installation"
     tool name: 'gradle', type: 'gradle'
   }
   stage('Checkout') { 
         checkout scm
        		workspace = pwd ()
			commit_username=sh(returnStdout: true, script: '''username=$(git log -1 --pretty=%an) 
                                                            echo ${username%@*} ''').trim();
			commit_username=sh(returnStdout: true, script: """echo ${commit_username} | sed 's/48236651+//g'""").trim()
			commit_Email=sh(returnStdout: true, script: '''Email=$(git log -1 --pretty=%ae) 
                                                            echo $Email''').trim();
			props = readProperties  file: """jenkinsJob.properties"""
			gituserName=sh(returnStdout: true, script: """echo \$(dirname ${apiRepoURL.trim()})""").trim();
			gituserName=sh(returnStdout: true, script: """echo \$(basename ${gituserName.trim()})""").trim();
			sh"""echo ${gituserName}""" 
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
        		app.push("${BUILD_NUMBER}")
        		app.push("latest")
      	}
   }
   stage('Run Container') {
      sh "docker run -p 8082:8080 -d paulsoumi96/test"
   }
}
