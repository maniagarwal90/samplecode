properties([
  parameters([
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
			sh 'cd $WORKSPACE/target/'
			sh 'ls -ltr'
			sh 'mv Jenkinsfile `date +"%d-%m-%Y-%H.%M"`_Jenkinsfile'
			sh 'scp -r *_Jenkinsfile user@server:${remote_dir_path}'
		}
	    }
	}
}
}
