properties([parameters([string(defaultValue: 'https://github.com/paulsoumi96/test.git', description: '', name: 'gitUrl', trim: false), string(defaultValue: 'master', description: '', name: 'gitBranch', trim: false)])])
node {
def app
def commit_username
def commit_email
def gituserName
   stage('Installations'){
      echo "gradle installation"
     tool name: 'gradle', type: 'gradle'
   }
   stage('Checkout') { 
	   git branch: "${gitBranch}", url: "${gitUrl}"
	 props = readProperties  file: """jenkinsJob.properties"""
         workspace = pwd ()
	 commit_username=sh(returnStdout: true, script: '''username=$(git log -1 --pretty=%an) 
                                                            echo ${username%@*} ''').trim();
	 commit_username=sh(returnStdout: true, script: """echo ${commit_username} | sed 's/48236651+//g'""").trim()
	 commit_Email=sh(returnStdout: true, script: '''Email=$(git log -1 --pretty=%ae) 
                                                            echo $Email''').trim();
	 gituserName=sh(returnStdout: true, script: """echo \$(dirname ${gitUrl.trim()})""").trim();
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
	   withCredentials([usernameColonPassword(credentialsId: "${props['docker.cred']}", variable: dc)]) 
			{
	   docker.withRegistry('https://registry.hub.docker.com',dc) {
        		app.push("${BUILD_NUMBER}")
        		app.push("latest")
      	}
			}
   }
   stage('Run Container') {
      sh "docker run -p 8082:8080 -d ${props['docker.image']}"
   }
}
