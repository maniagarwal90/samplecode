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
			sh 'mv samplecode-1.0.0.jar `date +"%d-%m-%Y-%H.%M"`samplecode-1.0.0.jar'
			sh 'scp -r * user@server:${remote_dir_path}'
		}
	    }
	}
}
}
