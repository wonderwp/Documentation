import groovy.json.JsonSlurperClassic

@NonCPS
def parseJsonToMap(String json) {
    final slurper = new JsonSlurperClassic()
    return new HashMap<>(slurper.parseText(json))
}

def loadCreds(){

        script {
            MY_SECRET_FILE_CONTENT = sh (
                script: "cat ${wonderwp_doc_credentials}",
                returnStdout: true
            ).trim()
            return parseJsonToMap(MY_SECRET_FILE_CONTENT)
        }

}

def handleException() {
	env.slackColor = "danger";
    def changeString_ = "${JOB_NAME}${env.BUILD_DISPLAY_NAME}\n";
    changeString_ += " - Build failed"
	env.slackMsg = changeString_;
}

def deployCode(creds) {
    echo "Sending files to remote server"
    sh "rsync -uvr --delete ${WORKSPACE}/doc/* ${creds.sshUser}@${creds.sshServer}:${creds.sshRemotePath};"
}

@NonCPS
def getChangeString() {
    MAX_MSG_LEN = 100
    def changeString = "${JOB_NAME}${env.BUILD_DISPLAY_NAME}\n"

    def changeLogSets = currentBuild.rawBuild.changeSets
    for (int i = 0; i < changeLogSets.size(); i++) {
        def entries = changeLogSets[i].items
        for (int j = 0; j < entries.length; j++) {
            def entry = entries[j]
            truncated_msg = entry.msg.take(MAX_MSG_LEN)
            changeString += " - ${truncated_msg} [${entry.author}]\n"
        }
    }

    if (!changeLogSets) {
        changeString += " - No new changes"
    }
    return changeString
}

def defineVariables(){
	env.slackColor = "good";
	env.slackMsg = getChangeString();
	env.runComposer = true;
}


pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script{
        	defineVariables();

            if(env.runComposer=='true'){
	            sh 'composer install';
	        } else {
	        	echo 'skipped composer install';
	        }

	        sh 'vendor/bin/daux --source=src/ --destination=doc --delete';
        }
      }
    }
    stage('deploy documentation') {
        steps {
            script {
            	try{
	                echo "Deploying Documentation"
	                def creds = loadCreds();
	                deployCode(creds);
	            } catch(exc){
	            	handleException();
                }
            }
        }
    }
    stage('notify'){
        steps {
            script {
    			slackSend(message: env.slackMsg, channel: '#jenkins', color: env.slackColor, failOnError: true, teamDomain: 'wdf-team', token: 'ebmZ6kgsvWsgFVUY3UViZPOS');
            }
        }
    }
  }
}
