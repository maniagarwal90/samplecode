properties([
  parameters([
    string(name: 'path_to_file', defaultValue: '$WORKSPACE/target/', description: ''),
    string(name: 'file_name', defaultValue: 'samplecode-1.0.0.jar', description: ''),
    string(name: 'remote_dir_path', defaultValue: '', description: 'enter the path to save file'),
   ])
])

pipeline  {
    agent any
	tools {
		maven "Maven 3.5.4"
	}
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')
    }
    environment { 
        REPOSITORY = 'https://github.com/maniagarwal90/samplecode.git'
        BRANCH = 'master'
        BUILDVERSION = 'sh(script: "echo `date +%s`", returnStdout: true).trim()'
	}
	stages {
		stage("Checkout code from SCM") {
		    steps {
		    	git branch: "${BRANCH}",
                url: "${REPOSITORY}"
		    }
		}
		stage("Build and Publish") {
		    steps {
			    script {
				    sh "mvn clean install"
			    }
		    }
		}
        stage('SSH transfer') {
            steps {
                script {
                sshPublisher(
                continueOnError: false, failOnError: true,
                publishers: [
                    sshPublisherDesc(
                    configName: "${env.SSH_CONFIG_NAME}",
                    verbose: true,
                    transfers: [
                    sshTransfer(
                    sourceFiles: "${path_to_file}/${file_name}",
                    removePrefix: "${path_to_file}",
                    remoteDirectory: "${remote_dir_path}",
                    execCommand: "run commands after copy?"
                    )
                    ])
                ])
                }
                }
	}
}
}
