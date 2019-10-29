node {
   def app
   stage('Installations'){
      echo "gradle installation"
     tool name: 'gradle', type: 'gradle'
   }
   stage('Checkout') { 
	 props = readProperties  file: """jenkinsJob.properties"""
       	 git branch: "${props['git.branch']}", url: "${props['git.url']}"
         workspace = pwd ()
	 commit_username=sh(returnStdout: true, script: '''username=$(git log -1 --pretty=%an) 
                                                            echo ${username%@*} ''').trim();
	 commit_username=sh(returnStdout: true, script: """echo ${commit_username} | sed 's/48236651+//g'""").trim()
	 commit_Email=sh(returnStdout: true, script: '''Email=$(git log -1 --pretty=%ae) 
                                                            echo $Email''').trim();
	 gituserName=sh(returnStdout: true, script: """echo \$(dirname ${apiRepoURL.trim()})""").trim();
	 gituserName=sh(returnStdout: true, script: """echo \$(basename ${gituserName.trim()})""").trim();
			sh"""echo ${gituserName}""" 
   }
   stage ('Build') {
      echo "initialization complete"
      sh './gradlew build'
   }
   stage ('Docker Image Build') {
       app = docker.build("${props['docker.image']}:${BUILD_NUMBER}")
   }
   stage ('Push Docker Image') {
	   docker.withRegistry('https://registry.hub.docker.com',"${props['docker.cred']}") {
        		app.push("${BUILD_NUMBER}")
        		app.push("latest")
      	}
   }
   stage('Run Container') {
      sh "docker run -p 8082:8080 -d ${props['docker.image']}"
   }
}
